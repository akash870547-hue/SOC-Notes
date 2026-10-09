# Day 04 — Network Security Architecture & Devices (Firewalls, IDS/IPS, WAF, Proxies & DMZ)

> **What you'll learn / Is chapter mein:** Compare perimeter security controls and learn how to reason from firewall, IDS/IPS, WAF and proxy logs.
>
> **Seedhi baat:** Firewall ka block event useful evidence hai, lekin woh apne aap na compromise prove karta hai na complete safety. Direction aur nearby logs correlate karo.

**Course focus:** Perimeter Defense, Network Telemetry Triage & Traffic Filtering

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. Firewalls: Evolution, Types & Inspection Engines

Firewall network perimeter ka first line of defense hota hai. Ye predefined security rulesets ke base par incoming aur outgoing network traffic ko permit (allow) ya deny (block/drop) karta hai.

```
Firewall Architecture Generations:
[ 1st Gen: Stateless / Packet Filter ] ──► [ 2nd Gen: Stateful Inspection ] ──► [ 3rd Gen: NGFW (App-Aware) ]
(Checks IP & Port in isolation)             (Tracks TCP Connection States)       (Layer 7 Inspection + IPS + SSL Decrypt)

```

### Types of Firewalls

#### A. Stateless / Packet-Filtering Firewall (Layer 3 & 4)

* **English:** Evaluates incoming and outgoing packets in isolation based purely on static Layer 3/4 header fields (Source IP, Destination IP, Source Port, Destination Port, Protocol) without maintaining state awareness of previous packets.
* **Hinglish:** Ye har packet ko bilkul alag (isolated) treat karta hai. Isko ye pata nahi hota ki packet kisi existing valid conversation ka part hai ya naya connection hai. Isme attacker easily IP spoofing aur out-of-order packets inject karke firewall bypass kar sakta hai.

#### B. Stateful Inspection Firewall (Layer 3 & 4)

* **English:** Maintains a dynamic State Table in memory that tracks active, established transport layer connections (e.g., TCP SYN, SYN-ACK, ACK states, session timers).
* **Hinglish:** Ye state table maintain karta hai. Agar ek internal client ne bahar internet par request bheji, toh firewall us outgoing connection ki entry state table mein save kar leta hai. Jab server se return traffic aata hai, firewall use automatically allow kar deta hai bina reverse rule likhe. Koi unauthorized incoming packet bina valid state table entry ke direct block ho jata hai.

#### C. Next-Generation Firewall - NGFW (Layer 7 / Application Layer)

* **Vendors:** Palo Alto Networks, Fortinet FortiGate, Check Point.
* **English:** Combines traditional stateful inspection with deep packet inspection (DPI), Layer 7 application identification (App-ID), integrated Intrusion Prevention Systems (IPS), SSL/TLS decryption, and real-time threat intelligence feeds.
* **Hinglish:** Normal firewall sirf port dekhta hai (e.g., Port 80 = allow). Lekin agar attacker Port 80 ya 443 par backdoor shell ya BitTorrent chala raha ho, toh normal firewall confuse ho jata hai. NGFW packet ke data payload ko inspect karke real application identify karta hai chahe wo kisi bhi non-standard port par chal rahi ho.

#### D. Web Application Firewall - WAF (Layer 7 Specialized)

* **Vendors:** Cloudflare, Imperva, AWS WAF, F5 BIG-IP.
* **Focus:** Web servers aur web APIs ko protect karna (OWASP Top 10 vulnerabilities: SQL Injection, Cross-Site Scripting / XSS, Remote File Inclusion).
* **Difference from NGFW:** NGFW pure enterprise network perimeter ko protect karta hai, jabki WAF specific HTTP/HTTPS applications ke application logic aur parameter inputs ko inspect karta hai.

---

## 2. Intrusion Detection vs. Intrusion Prevention Systems (IDS / IPS)

```
       [ Internet Traffic ]
                │
                ▼
      ┌──────────────────┐
      │   Inline (IPS)   │ ──► [ Drops Malicious Packet ] ──► Alerts SIEM
      └─────────┬────────┘
                │ Legitimate Traffic
                ▼
      ┌──────────────────┐
      │ Internal Network │
      └──────────────────┘
                │ (SPAN / TAP Port Mirroring)
                ▼
      ┌──────────────────┐
      │ Out-of-Band(IDS) │ ──► [ Passive Detection Only ] ──► Alerts SIEM
      └──────────────────┘

```

| Parameter | Intrusion Detection System (IDS) | Intrusion Prevention System (IPS) |
| --- | --- | --- |
| **Placement** | **Out-of-Band (Passive):** Switch ke SPAN/TAP port se traffic ki copy receive karta hai. | **Inline:** Direct live traffic path mein baithta hai (all traffic flows through it). |
| **Action on Threat** | Sirf alert generate karta hai (`Alert-Only`). Attack packet ko drop nahi kar sakta. | Alert generate karta hai aur packet ko real-time mein drop/block kar deta hai. |
| **Network Latency** | Zero latency (Network speed par koi asar nahi padta). | Slight latency add ho sakti hai kyunki har packet ko process karke forward karna padta hai. |
| **False Positive Risk** | Low impact (Sirf false alarm aayega, legitimate traffic block nahi hoga). | High impact (Agar legitimate corporate traffic false positive ban gaya toh business disrupt ho sakta hai). |

### Detection Methodologies

1. **Signature-Based Detection:**
* Known threat patterns (hashes, specific string sequences, known exploit byte patterns) se traffic match karta hai.
* *Advantage:* Highly accurate for known attacks; minimal false positives.
* *Limitation:* Zero-Day exploits aur polymorphic malware ko detect nahi kar pata.

2. **Anomaly / Behavior-Based Detection:**
* Pehle network ka "normal baseline" establish karta hai (e.g., regular bandwidth, standard ports, typical traffic hours). Jab baseline se statistically significant deviation hota hai, tab alert raise karta hai.
* *Advantage:* Zero-day exploits aur unknown threats detect kar sakta hai.
* *Limitation:* High false positive rate (e.g., sudden genuine bulk data migration ko attack samajh lena).

---

## 3. Network Architecture: DMZ, VLANs & Bastion Hosts

```
                            [ Internet (Untrusted) ]
                                       │
                                       ▼
                             ┌──────────────────┐
                             │ External Firewall│
                             └─────────┬────────┘
                                       │
                ┌──────────────────────┴──────────────────────┐
                │                                             │
                ▼                                             ▼
     ┌──────────────────────┐                      ┌──────────────────────┐
     │   DMZ (Perimeter)    │                      │  Internal Firewall   │
     │ Web / Mail / DNS     │                      └──────────┬───────────┘
     └──────────────────────┘                                 │
                                                              ▼
                                                   ┌──────────────────────┐
                                                   │   Internal Network   │
                                                   │ AD / DB / Workstation│
                                                   └──────────────────────┘

```

### DMZ (Demilitarized Zone)

* **English:** A physical or logical subnetwork that separates an internal local area network (LAN) from untrusted external networks (the Internet). Public-facing services reside here.
* **Hinglish:** Ye ek buffer zone hota hai jisme public-facing servers (Web server, Public DNS server, External Mail gateway) rakhe jaate hain. Agar hacker internet se kisi web server ko exploit kar bhi leta hai, toh wo directly internal company network (Finance, HR, Active Directory) mein jump nahi kar sakta kyunki internal firewall DMZ se internal LAN ki taraf traffic strictly block karta hai.

### Network Segmentation & VLANs (Virtual Local Area Networks)

* Flat networks sabse bada security hazard hote hain (agar ek receptionist ka laptop infect hua toh hacker direct Domain Controller tak pahunch sakta hai).
* **Micro-segmentation:** Network ko isolated zones mein divide karna:
* VLAN 10: User Workstations
* VLAN 20: Production Servers
* VLAN 30: Database Servers (Strictly isolated; accessible only from App servers via port 1433/3306)
* VLAN 40: Management Network (SSH/RDP access restricted via Jump Server)

### Bastion Host / Jump Box

* Ek hardened server jiske through administrative staff secure internal servers (Production environment) ko access karte hain. Direct administrative access (SSH/RDP) external internet se completely forbidden hota hai.

---

## 4. Proxies & SSL/TLS Decryption

### Forward Proxy vs. Reverse Proxy

* **Forward Proxy:** Internal corporate users ke behalf par internet se resources fetch karta hai. Iska use URL filtering, employee content control aur caching ke liye hota hai.
* **Reverse Proxy:** External internet users ke requests ko receive karta hai aur unhe internal backend web servers par route karta hai. Iska use Load Balancing, Web Acceleration aur DDoS mitigation ke liye hota hai.

### SSL/TLS Decryption (Break-and-Inspect)

* Enterprise web traffic ka 85%+ part HTTPS encrypted hota hai. Attackers malicious payloads aur C2 commands ko SSL/TLS encryption ke peeche chupakar corporate perimeter se pass karwa dete hain.
* **Corporate SSL Inspection Workflow:**
1. Internal client machine browser mein `[https://external-site.com](https://external-site.com)` access karti hai.
2. Firewall/Proxy connection intercept karta hai.
3. Firewall khud external site ke sath alag secure session banata hai aur remote SSL certificate verify karta hai.
4. Firewall traffic ko decrypt karke inspect karta hai (antivirus/IPS scan run karta hai).
5. Content clean hone par firewall internal corporate Root CA certificate se re-encrypt karke packet client tak bhej deta hai.

---

## 5. SOC Analyst Firewall Log Triage

Jab SIEM mein firewall alert aata hai, toh analyst log entry ke essential fields ko analyze karta hai:

```
Sample Firewall Syslog:
2026-10-09T10:14:22Z FW01-EDGE %SEC-6-LOG: action=DENY proto=TCP src=203.0.113.88 sport=49182
dst=10.0.1.15 dport=445 reason="Rule_Drop_SMB_Inbound" rule_id=104

```

### Critical Fields to Correlate

1. **Action (PERMIT / DENY / REJECT):**
* `DENY/DROP`: Packet silently drop kar diya gaya (best practice for external attacks).
* `REJECT`: Packet drop kiya gaya aur source ko RST/ICMP Port Unreachable packet wapas bheja gaya (reveals firewall presence).

2. **Direction (Inbound vs Outbound):**
* *Inbound Drops:* Normal internet noise ya automated scanning (Low priority unless targeting critical asset).
* *Outbound Drops:* Highly critical! Internal endpoint kisi malicious external IP se connect hone ki koshish kar raha hai jo firewall rule dwara block hui (Possible malware infection / C2 attempt).

3. **Port & Protocol Mismatch:** Non-standard ports par critical protocols ka chalna.

---

## 6. High-Yield Interview Q&A (Day 4 Focus)

**Q1: What is the difference between a Drop and a Reject action on a firewall? Which one is preferred from a security standpoint?**

* **Answer:**
* **Drop:** Firewall incoming unauthorized packet ko silently discard kar deta hai aur sender ko koi notification ya return packet nahi bhejta. Attacker ko lagta hai ki host down hai ya network blackhole hai.
* **Reject:** Firewall packet ko discard karta hai aur sender ko explicitly TCP RST (Reset) ya ICMP Unreachable packet return karta hai.
* **Preference:** Perimeter defense ke liye **DROP** prefer kiya jata hai kyunki ye attacker ko reconnaissance data nahi deta aur firewall processing overhead kam karta hai.

**Q2: What is a WAF bypass technique, and how should a SOC analyst respond?**

* **Answer:** Attackers SQL Injection ya XSS payloads ko encode (e.g., Double URL Encoding, Hex encoding, Unicode, Case manipulation `sElEcT`) karke WAF ke regex filters ko evade karne ki koshish karte hain. SOC analyst ko web server access logs aur database query logs check karke dekhna hota hai ki kya backend database ne payload decode karke actually execute kiya (`HTTP 200 / 500 error`) ya application logic ne use discard kar diya (`HTTP 403 / 400`).

---

---

---

## Analyst takeaway / Yaad rakhne wali baat

Controls ke action (allow/drop/inspect) ko asset criticality aur downstream logs ke saath interpret karo.

## Practice prompt / Khud try karo

Sample firewall DENY event ko classify karo: direction, target asset, blocked service, adjacent evidence aur closure/escalation rationale.
