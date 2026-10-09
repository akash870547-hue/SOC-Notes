# Day 05 — Cyber Threat Intelligence (CTI), Indicators of Compromise (IOCs) & Pyramid of Pain

> **What you'll learn / Is chapter mein:** Enrich indicators responsibly, understand IOC vs IOA, and use the Pyramid of Pain as a prioritization model.
>
> **Seedhi baat:** Reputation lookup ko final verdict mat samjho. Indicator ko internal telemetry, freshness, context aur benign explanations se verify karo.

**Course focus:** Threat Enrichment, IOC Lifecycle & TTP-Based Hunting

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. What is Cyber Threat Intelligence (CTI)?

### Conceptual Definition

* **English:** Cyber Threat Intelligence (CTI) is evidence-based knowledge—including context, mechanisms, indicators, implications, and actionable advice—about an existing or emerging menace or hazard to assets.
* **Hinglish:** CTI sirf raw data ya random IP addresses ki list nahi hai. Ye processed aur analyzed information hoti hai jo ye explain karti hai ki attacker kaun hai (Threat Actor), unka motivation kya hai, wo kaunse tools aur techniques (TTPs) use karte hain, aur organization unke attacks ko proactively kaise detect aur defend kar sakti hai.

```
Raw Data ────────► Information ────────► Intelligence
(Single IP)        (IP linked to         (IP belongs to LockBit RaaS; targets
                    malicious domain)     banking sector using CVE-2024-XXXX)

```

### The 4 Levels of Threat Intelligence

```
┌────────────────────────────────────────────────────────┐
│ 1. Strategic CTI  (Executives, CISOs, Board Level)     │
├────────────────────────────────────────────────────────┤
│ 2. Operational CTI (Threat Hunters, Incident Responders)│
├────────────────────────────────────────────────────────┤
│ 3. Tactical CTI   (Architects, Detection Engineers)    │
├────────────────────────────────────────────────────────┤
│ 4. Technical CTI  (SOC L1 Analysts - IOC Feeds)        │
└────────────────────────────────────────────────────────┘

```

1. **Strategic CTI:** High-level trends, geopolitical cyber risks, financial loss estimations. (Used for security budget allocation and executive decision making).
2. **Operational CTI:** Specific threat actor profiles, campaigns, and adversary capabilities. (Used by incident responders to anticipate the attacker's next move during a live breach).
3. **Tactical CTI:** Specific adversary TTPs mapped to MITRE ATT&CK. (Used to build custom SIEM correlation rules and EDR behavioral signatures).
4. **Technical CTI:** Consumable technical artifacts like malicious IPs, domains, URL endpoints, and file hashes. (Directly ingested into SIEM/EDR blocklists).

---

## 2. IOCs vs. IOAs (Core Conceptual Distinction)

```
┌──────────────────────────────────────────────┐
│ Indicator of Compromise (IOC)                │
│ "What the attacker used in the past"         │
│ (Reactive / Forensic Artifacts)              │
│ Examples: Hash, Malicious IP, C2 Domain      │
└──────────────────────┬───────────────────────┘
                       │ vs.
┌──────────────────────▼───────────────────────┐
│ Indicator of Attack (IOA)                    │
│ "What the attacker is actively doing right now"│
│ (Proactive / Behavioral Intent)              │
│ Examples: LSASS memory access, Encoded PS    │
└──────────────────────────────────────────────┘

```

* **Indicator of Compromise (IOC):**
* *Nature:* Reactive / Post-Exploitation forensic artifact.
* *Scenario:* Endpoint scan mein ek file milti hai jiska SHA-256 hash `d41d8cd98f...` hai, jo VirusTotal par known Emotet malware ke roop mein flagged hai.
* *Limitation:* Attacker payload ka single bit change karke hash completely badal sakta hai (Hash collision avoidance).

* **Indicator of Attack (IOA):**
* *Nature:* Real-time Behavioral / Pre-breach telemetry.
* *Scenario:* Ek MS Excel file open hoti hai, jo bina kisi disk write ke system memory mein reflective DLL injection perform karti hai aur `cmd.exe` spawn karti hai.
* *Advantage:* Malware chahe completely new ho (Zero-Day) aur uska hash kisi database mein na ho, fir bhi uska suspicious behavior alert trigger kar deta hai.

---

## 3. David Bianco's Pyramid of Pain

David Bianco ne ek model design kiya jo measure karta hai ki jab defense team kisi specific artifact ko detect ya block karti hai, toh usse attacker ko kitna operational nuksan (pain) hota hai.

```
               ▲
              / \
             /   \      TTPs (Tactics, Techniques, Procedures)  [TOUGH]
            / TTP \
           /───────\
          /  Tools  \   Malicious Utilities (Mimikatz, Cobalt)  [CHALLENGING]
         /───────────\
        / Host/Network\ Artifacts (Registry keys, User-Agents)  [ANNOYING]
       /───────────────\
      /  Domain Names   \ C2 Domains, Dynamic DNS               [SIMPLE]
     /───────────────────\
    /     IP Addresses    \ Public Proxy / Fast-Flux IPs        [EASY]
   /───────────────────────\
  /       Hash Values       \ SHA-256, MD5, SHA-1               [TRIVIAL]
 /───────────────────────────\

```

### Deep Dive into Pyramid Layers (From Base to Peak)

| Level | Indicator Type | Attacker Pain to Change | Defender Impact & Reality |
| --- | --- | --- | --- |
| **1. Hash Values** | MD5, SHA-1, SHA-256 of files. | **Trivial (Zero Effort):** Attacker single null-byte add karke ya recompiling karke completely naya hash generate kar sakta hai. | Useful for rapid automated blacklisting, but has shortest shelf-life. |
| **2. IP Addresses** | Attacker source IPs, C2 listener IPs. | **Easy:** Cloud providers (AWS, Azure) ya Tor/VPNs ka use karke attacker seconds mein naya IP acquire kar sakta hai. | IP blocking helps temporarily, but causes high false positives (shared hosting IPs). |
| **3. Domain Names** | Malicious websites, DGA domains. | **Simple:** Bulletproof registrars se new cheap domains buy karna ya Domain Generation Algorithms (DGA) use karna. | DNS sinkholing aur WHOIS domain age checks are effective countermeasures. |
| **4. Network / Host Artifacts** | Specific URI paths, Registry autorun values, unique HTTP User-Agents. | **Annoying:** Attacker ko apne code ke configuration parameters modify karne padte hain. | High detection fidelity (e.g., Custom User-Agent strings used by older malware tools). |
| **5. Tools** | Specific malware utilities (Cobalt Strike, PsExec, Mimikatz, Metasploit). | **Challenging:** Attacker ko naya tool create karna padega ya existing tool ko heavily re-engineer / obfuscate karna padega. | EDR behavioral blocking of known offensive utilities. |
| **6. TTPs** | Fundamental attack techniques (e.g., Pass-the-Hash, Process Hollowing, Kerberoasting). | **Tough (Maximum Pain):** Attacker ko apni core methodologies, skills, aur operational training abandon karke nayi techniques seekhni padti hain. | The ultimate goal of modern threat detection and hunting! |

---

## 4. Threat Intelligence Enrichment Platforms & Tools

SOC L1 analyst alert triage karte waqt har external artifact ko standard threat intel engines par enrich karta hai:

```
Alert Ingested (Suspicious Hash / IP / Domain)
         │
         ├── File Hash ────────► VirusTotal, Hybrid-Analysis, ANY.RUN
         ├── IP Address ───────► AbuseIPDB, Shodan, Cisco Talos, GreyNoise
         └── Domain / URL ─────► URLScan.io, WHOIS, AlienVault OTX

```

### Key Enrichment Engines

* **VirusTotal:** Aggregates 70+ antivirus engines and domain blocklists. Check detections count, execution behavior, dropped files, and associated network communications.
* **AbuseIPDB:** Community-driven IP reputation database. Shows abuse confidence score and historical reporting categories (DDoS, port scanning, SSH brute-force).
* **Shodan:** Search engine for internet-connected devices. Reveals open ports, running services, SSL certificate details, and known vulnerabilities on an external IP.
* **GreyNoise:** Differentiates between targeted attacks and harmless "internet background noise" (e.g., Shodan scanners, benign security research crawlers).
* **AlienVault OTX (Open Threat Exchange):** Community-driven platform where security researchers share "Pulses" (curated collections of IOCs associated with specific threat campaigns).
* **MISP (Malware Information Sharing Platform):** Open-source threat intelligence sharing platform used by enterprises and CERT teams to share structured threat data.

---

## 5. Integrating CTI into SIEM Workflows (STIX & TAXII)

Automated threat intelligence manual lookups ko eliminate karta hai through standardized threat communication protocols:

* **STIX (Structured Threat Information eXpression):**
* A structured JSON-based language used to describe cyber threat information (Threat Actors, Campaigns, TTPs, Observables, Vulnerabilities) in a machine-readable format.

* **TAXII (Trusted Automated eXchange of Intelligence Information):**
* The application layer protocol (transport mechanism over HTTPS) used to securely exchange STIX-formatted threat intelligence between systems.

```
[ External CTI Feed / ISAC ] ──(TAXII Protocol over HTTPS / STIX JSON)──► [ Enterprise SIEM / TIP ]
                                                                                   │
                                                                       Auto-Match Incoming Logs
                                                                       with Ingested Threat IOCs

```

---

## 6. High-Yield Interview Q&A (Day 5 Focus)

**Q1: If you find an external IP with a 100% Abuse Confidence Score on AbuseIPDB that has initiated 50 connection attempts to your perimeter firewall, but all connections were dropped by the firewall, what is your triage verdict and next action?**

* **Answer:**
* **Triage Verdict:** True Positive (Malicious scan activity), but **No Impact / Successfully Defended** (Benign True Positive).
* **Reasoning:** Inbound drops show that perimeter defenses functioned as intended. The malicious IP failed to breach the network.
* **Next Action:** Verify that no successful outbound connection from any internal endpoint was made to that IP during the same time window. If the scan is part of a broad internet sweep, close the ticket as handled. If high frequency targets a specific asset, verify IP blocklist duration and document the incident.

**Q2: Why is blocking file hashes considered the lowest level in the Pyramid of Pain?**

* **Answer:** Modern malware authors use techniques like compilation timestamps, changing icon resources, dead-code insertion, or automated crypters/packers. These techniques alter the file's binary structure without changing its core malicious behavior, resulting in completely different MD5/SHA-256 hashes for every generated payload. Blocking static hashes provides zero protection against new variants of the exact same malware.

---

---

## Analyst takeaway / Yaad rakhne wali baat

IOC useful lead hai, final answer nahi; freshness, provenance aur internal sightings record karo.

## Practice prompt / Khud try karo

Ek IOC enrichment note banao jisme source, timestamp, confidence, internal sightings aur expiry/review date ho.
