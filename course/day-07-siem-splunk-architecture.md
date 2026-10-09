# Day 07 — SIEM Architecture, Working Principles & Splunk Enterprise Fundamentals

> **What you'll learn / Is chapter mein:** Learn SIEM ingestion, normalization, correlation, Splunk components and the data lifecycle.
>
> **Seedhi baat:** SIEM sirf logs ka storage nahi: analyst ko samajhna hota hai ki event kahan se aaya, fields kaise bane, aur rule kyun fire hua.

**Course focus:** Log Ingestion Pipeline, Splunk Core Components & Data Normalization

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. What is a SIEM (Security Information and Event Management)?

### Conceptual Definition

* **English:** A SIEM is a centralized security platform that aggregates, normalizes, correlates, and analyzes machine-generated log data from across an enterprise IT environment in real time to detect security incidents, satisfy compliance requirements, and support forensic investigations.
* **Hinglish:** SIEM ek centralized engine hota hai jo company ke pure infrastructure (Firewalls, Windows Servers, Linux Endpoints, Cloud Environments, Antivirus, Routers) se aane wale logs ko ek jagah collect (aggregate) karta hai, unhe parse karke standard format mein convert (normalize) karta hai, alag-alag sources ke events ko aapas mein jodta hai (correlate), aur suspicious pattern milne par alert raise karta hai.

```
SIEM Core Lifecycle:
[ Raw Logs ] ──► [ Ingestion ] ──► [ Normalization ] ──► [ Correlation ] ──► [ Alerting / Storage ]
(Syslog/API)      (Forwarders)     (Field Extraction)    (Rule Matching)     (SIEM Alert Queue)

```

### Core Functions of a Modern SIEM

1. **Log Aggregation:** Har tarah ke source (Syslog, Windows Event Forwarding, REST APIs, Flat files) se logs collect karna.
2. **Parsing & Normalization:** Alag-alag vendors ke log format ko ek standard format mein map karna (e.g., Check Point firewall log mein `src_ip`, Palo Alto mein `source_ip` ko common field name `src` banana).
3. **Correlation Engine:** Real-time rules match karna.
* *Example:* "Agar ek hi IP se 5 minute ke andar 20 failed logins (Event 4625) aate hain aur uske turant baad 1 successful login (Event 4624) aata hai, toh High-Severity Brute Force alert generate karo."

4. **Retention & Archival:** Compliance (PCI-DSS, ISO 27001) ke audit rules follow karne ke liye logs ko 90 days se lekar multiple years tak safely store rakhna.

---

## 2. Enterprise Splunk Architecture & Core Components

Industry standard SIEM platforms mein Splunk sabse widely deployed platform hai. Splunk distributed architecture teen main functional tiers par chalta hai:

```
[ Tier 1: Collection ]         [ Tier 2: Indexing & Storage ]         [ Tier 3: Search & Analytics ]

┌──────────────────────┐
│ Universal Forwarder  │ ──┐
└──────────────────────┘   │
                           ▼
┌──────────────────────┐  ┌───────────────────────────┐      ┌─────────────────────────────┐
│   Heavy Forwarder    │─►│      Indexer Cluster      │ ◄─── │      Search Head (SH)       │
└──────────────────────┘  │ (Stores data into buckets)│      │ (SOC Analysts run SPL here) │
                           └───────────────────────────┘      └─────────────────────────────┘
                                         ▲
                                         │ (Management & Configs)
                           ┌───────────────────────────┐
                           │ Deployment Server / Master│
                           └───────────────────────────┘

```

### Component Breakdown

| Splunk Component | Role & Functionality | SOC Architecture Insight |
| --- | --- | --- |
| **Universal Forwarder (UF)** | Lightweight software agent jo endpoints/servers par install hota hai. Raw logs collect karke indexer ko bhejta hai. | Zero parsing perform karta hai; minimal CPU/RAM consume karta hai. |
| **Heavy Forwarder (HF)** | Full Splunk instance jo data route aur parse kar sakta hai. Sensitive data (jaise passwords, credit card numbers) mask/filter karne ke liye use hota hai. | Unwanted logs (junk debug logs) ko drop karke Indexer licensing cost save karta hai. |
| **Indexer** | Splunk ka workhorse. Data ko parse karta hai, index create karta hai, aur disk par compressed format mein store karta hai. | Data ko time-based buckets mein divide karke fast retrieval ensure karta hai. |
| **Search Head (SH)** | Frontend Graphical Interface (GUI). SOC analysts apne queries (SPL) search head par run karte hain, jo indexers se data fetch karke visualize karta hai. | Dashboards, Alerts, aur SOC reports search head par maintain hote hain. |
| **Deployment Server (DS)** | Central management node jo sabhi forwarders ko configuration files (`inputs.conf`, `outputs.conf`) push karta hai. | Ek click par 5,000 forwarders par naya log path configure karne ke kaam aata hai. |

---

## 3. Splunk Indexing: The Bucket Lifecycle

Indexer data ko unke creation time ke according **Buckets** (directories) mein store karta hai. Data age badhne ke sath buckets state transition karti hain:

```
[ Raw Data Ingest ] ──► [ Hot Bucket ] ──► [ Warm Bucket ] ──► [ Cold Bucket ] ──► [ Frozen Bucket ]
                         (Read/Write)      (Read-Only)         (Slower Disk)      (Archived / Purged)

```

* **Hot Bucket:** Currently open for reading and writing. Fast NVMe/SSD storage par rehti hai.
* **Warm Bucket:** File size limit ya time reach hone par Hot bucket Warm bucket ban jati hai. Ye read-only hoti hai, isme naya data write nahi hota.
* **Cold Bucket:** Purana historical data. Cost optimize karne ke liye ise cheaper magnetic HDD ya network-attached storage par move kiya jata hai.
* **Frozen Bucket:** Retention policy reach hone par (e.g., 365 days) data frozen ban jata hai. Ye default search head se search nahi ho sakti jab tak ise thaw (restore) na kiya jaye.
* **Thawed Bucket:** Forensics investigation ke liye archive storage se manually restore ki gayi bucket.

---

## 4. The 4 Essential Splunk Metadata Fields

Splunk mein koi bhi log enter hote hi Splunk use 4 mandatory default fields assign karta hai:

```
_time  |  host  |  source  |  sourcetype  |  index

```

1. **`index`:** Data ka physical/logical boundary container (e.g., `index=windows`, `index=firewall`, `index=proxy`). SOC environments mein alag-alag teams aur data types ke alag indices hote hain taaki access control aur query performance fast rahe.
2. **`sourcetype`:** Splunk ko batata hai ki log ko kaise format aur parse karna hai (e.g., `sourcetype=WinEventLog:Security`, `sourcetype=cisco:asa`, `sourcetype=access_combined`).
3. **`source`:** File ka exact path ya network stream jahan se log generate hua (e.g., `/var/log/secure`, `C:\Windows\System32\winevt\Logs\Security.evtx`, `UDP:514`).
4. **`host`:** Machine ya device ka physical host name ya IP jahan se log generate hua (e.g., `WKS-FINANCE-01`, `DC-PROD-01`).

---

## 5. CIM (Common Information Model) & Normalization

Enterprises multiple vendors use karte hain (Palo Alto, Fortinet, Cisco, Windows, Linux). Har vendor log field names alag likhta hai:

```
Palo Alto Log:  src_ip="192.168.1.10"  dst_ip="8.8.8.8"
Cisco ASA Log:  source_address="192.168.1.10"  dest_address="8.8.8.8"
Windows Log:    IpAddress="192.168.1.10"  TargetAddress="8.8.8.8"

```

* **Without CIM:** Analyst ko ek single investigation ke liye 3 alag queries likhni padengi.
* **With Splunk CIM (Data Normalization):** Splunk Field Aliasing aur Calculated Fields ke through in sabhi vendor fields ko ek standardized schema name par map kar deta hai:
* Normalized Field: `src = "192.168.1.10"`
* Normalized Field: `dest = "8.8.8.8"`

* SOC analyst sirf `src="192.168.1.10"` search karta hai aur pure enterprise ke kisi bhi vendor log ka traffic instant correlate ho jata hai.

---

## 6. High-Yield Interview Q&A (Day 7 Focus)

**Q1: What is the technical difference between a Universal Forwarder (UF) and a Heavy Forwarder (HF) in Splunk? When should you deploy each?**

* **Answer:**
* **Universal Forwarder (UF):** C-based lightweight agent hai jo raw log collection karta hai bina data ko parse kiye. Endpoints, application servers aur workstations par deploy kiya jata hai jahan low CPU/RAM footprint requirement hoti hai.
* **Heavy Forwarder (HF):** Full enterprise Splunk installation hai jo Python interpreter aur full parsing engine run karta hai. Ye data routing, complex filtering, indexing se pehle sensitive data (PII/Card details) anonymize/mask karne, ya non-standard APIs se data pull karne ke liye intermediate collection tier par deploy kiya jata hai.

**Q2: Why is it considered a poor practice to search without specifying an `index` in Splunk (e.g., searching `error` directly)?**

* **Answer:** Agar query mein `index` specify nahi kiya jata, toh Splunk default indices (`index=default` ya user-accessible all indices) ke har single bucket ko scan karta hai. Isse Indexer I/O overhead exponentially badh jata hai, search execution slow ho jati hai, aur hardware resources waste hote hain. SOC rule of thumb hai: **Hamesha search query ko narrow `index` aur `sourcetype` se start karo.**

---

---

---

## Analyst takeaway / Yaad rakhne wali baat

Data onboarding aur field normalization galat ho to detection bhi unreliable hogi. Raw event aur extracted fields dono inspect karo.

## Practice prompt / Khud try karo

Ek sample log source ke ingestion se alert tak field mapping aur quality checks diagram karo.
