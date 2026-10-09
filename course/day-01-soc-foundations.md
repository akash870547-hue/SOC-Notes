# Day 01 — Introduction to SOC (Security Operations Center), Architecture, Roles & Lifecycle

> **What you'll learn / Is chapter mein:** Understand the SOC mission, team tiers, core tools, operational metrics and incident-response lifecycle.
>
> **Seedhi baat:** Seedhi baat: SOC ka kaam sirf alerts dekhna nahi—evidence ko samajhna, risk judge karna aur next analyst ko clean handoff dena hai.

**Course focus:** L1 SOC Analyst Fundamentals & Real-World Operations

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. What is a SOC (Security Operations Center)?

### Conceptual Definition

* **English:** A Security Operations Center (SOC) is a centralized function within an organization employing people, processes, and technology to continuously monitor, detect, analyze, and respond to cybersecurity incidents on a 24/7/365 basis.
* **Hinglish:** SOC ek centralized team aur facility hoti hai jiska primary goal organization ke pure IT infrastructure (endpoints, servers, network devices, cloud environments, applications) ko 24/7 monitor karna hota hai. Yahan aane wale threats ko detect karna, unhe analyze karna aur kisi bhi potential breach ya cyber attack ko actively prevent/respond karna SOC ka kaam hota hai.

### Core Objectives of a SOC

1. **Visibility & Asset Monitoring:** Pure enterprise infrastructure ke logs aur network telemetry ko centrally ingest karna.
2. **Proactive Threat Detection:** Malicious patterns, IOCs (Indicators of Compromise), aur anomalous behavior ko identify karna.
3. **Rapid Incident Response:** Cyber attack ke impact ko minimize karna through containment aur remediation.
4. **Compliance & Reporting:** Regulatory standards (PCI-DSS, ISO 27001, HIPAA, GDPR) ke according security audit trails maintain karna.

---

## 2. SOC vs NOC (Core Interview Difference)

Interviewers ka favorite basic question hota hai: *"What is the core difference between a NOC and a SOC?"*

| Parameter | NOC (Network Operations Center) | SOC (Security Operations Center) |
| --- | --- | --- |
| **Primary Goal** | Network Availability, Uptime aur Performance ensure karna. | Confidentiality, Integrity aur Availability (CIA Triad) ko defend karna. |
| **Focus Area** | Network downtime, link failures, latency, packet loss, bandwidth spikes. | Cyber attacks, unauthorized access, malware infections, data exfiltration. |
| **Typical Alerts** | "Router CPU at 95%", "Switch Port down", "Server unreachable". | "Brute-force attack detected", "Ransomware beaconing to C2", "Pass-the-Hash alert". |
| **Target SLA Metric** | MTTR (Mean Time to Repair/Restore service). | MTTD (Mean Time to Detect) & MTTR (Mean Time to Respond). |

---

## 3. The PPT Framework (People, Process, Technology)

Ek successful SOC teen pillars ke combination par chalta hai:

### A. People (Human Capital)

Automated tools sirf alerts generate karte hain, lekin un alerts ko interpret aur triage karne ke liye human intelligence chahiye.

* L1, L2, L3 Analysts
* Incident Responders
* Threat Hunters & Forensic Specialists
* SOC Leads & Managers

### B. Process (Standard Operating Procedures & Frameworks)

Bina standardized process ke chaos create ho jata hai jab live breach hoti hai.

* **Playbooks / Runbooks:** Specific attacks (e.g., Phishing, Malware Outbreak, DDoS) ke liye step-by-step SOPs.
* **Escalation Matrix:** Konsa alert kis tier ke paas kab escalate hoga.
* **Shift Handover Protocols:** 24/7 rotation mein outgoing shift incoming shift ko pending tickets transfer karti hai.

### C. Technology (Tooling Stack)

* **Data Aggregation & Correlation:** SIEM (Splunk, Microsoft Sentinel, IBM QRadar).
* **Endpoint Protection:** EDR/XDR (CrowdStrike Falcon, Microsoft Defender for Endpoint).
* **Network Visibility:** IDS/IPS (Suricata, Snort), NDR, Next-Gen Firewalls (Palo Alto, Fortinet).
* **Automation & Orchestration:** SOAR (Cortex XSOAR, Splunk SOAR).
* **Threat Intel & Ticketing:** AlienVault OTX, VirusTotal, ServiceNow, Jira.

---

## 4. SOC Tier Structure & Career Hierarchy

```
[ Tier 1 (L1) Analyst ]  ---> Alert Triage, True/False Positive Verification
         │ (Escalation)
[ Tier 2 (L2) Responder ] ---> Deep Investigation, Root Cause Analysis (RCA), Containment
         │ (Complex / APT)
[ Tier 3 (L3) Hunter/DFIR] ---> Proactive Threat Hunting, Reverse Engineering, Forensics
         │
[ SOC Lead / Manager ]   ---> Process Tuning, Metrics Reporting, Compliance

```

### Tier 1 (L1) – Alert Triage Analyst

* **Primary Role:** Frontline defender.
* **Responsibilities:**
* SIEM dashboard par continuous alert queue monitor karna.
* Initial triage perform karna: Check karna ki alert **False Positive (FP)** hai ya **True Positive (TP)**.
* Initial data collection: IP reputation check, hash analysis on VirusTotal, user behavior verify karna.
* Verified true positives ke liye proper context aur artifact add karke ticket create karna aur L2 ko escalate karna.

### Tier 2 (L2) – Incident Responder / Deep Investigator

* **Primary Role:** Technical investigation and remediation lead.
* **Responsibilities:**
* L1 se escalate hue incidents ka deep forensic and correlation analysis karna.
* **Scope Determination:** Attack kitna faila hai (lateral movement trace karna).
* Containment steps execute karna: Host isolation, firewall IP block, active directory account disable.
* Incident report draft karna aur **Root Cause Analysis (RCA)** ready karna.

### Tier 3 (L3) – Threat Hunter & Forensics Expert

* **Primary Role:** Advanced threat mitigation & proactive hunting.
* **Responsibilities:**
* System mein bina alert trigger hue chhupe advanced persistent threats (APTs) ko proactively hunt karna.
* Malware reverse engineering aur memory forensics perform karna.
* Detection engineering: Custom SIEM correlation rules, YARA rules, aur Sigma rules design karna.

### SOC Manager / Lead

* Overall SOC operations ko oversee karna, client SLAs maintain karna, executive leadership ko breach briefing dena aur continuous tool improvement ensure karna.

---

## 5. Critical SOC Performance Metrics (KPIs)

SOC ki effectiveness measure karne ke liye metrics use hote hain:

1. **MTTD (Mean Time to Detect):**
* Compromise hone ke baad analyst ya system ko use detect karne mein kitna average time laga. Goal: Isse minutes ya seconds tak lana.

2. **MTTR (Mean Time to Respond / Resolve):**
* Threat identify hone ke baad use isolate aur remediate karne mein kitna average time laga.

3. **Alert Fatigue:**
* Jab daily hazaron false positive alerts aate hain, toh analysts thak kar genuine critical alerts miss kar dete hain. Isko eliminate karne ke liye rule tuning zaroori hoti hai.

### Alert Classification Matrix

* **True Positive (TP):** Alert aaya aur sach mein attack ho raha tha. *(Correct Action: Investigate)*
* **False Positive (FP):** Alert aaya lekin actual traffic legitimate tha (e.g., admin ne authorized backup run kiya aur data exfiltration ka alert aa gaya). *(Correct Action: Rule Tune)*
* **True Negative (TN):** Normal traffic chal raha tha aur koi alert nahi aaya. *(Healthy State)*
* **False Negative (FN):** Actual attack hua par tool alert generate karne mein fail ho gaya. *(Worst-Case Scenario)*

---

## 6. Incident Response Lifecycle (NIST SP 800-61 Framework)

Har SOC analyst ko NIST Incident Handling lifecycle ke according chalna hota hai:

```
    ┌──────────────────────┐
    │     1. Preparation   │
    └──────────┬───────────┘
               ▼
    ┌──────────────────────┐
    │ 2. Detection & Analy.│ ◄─── (Most L1/L2 work happens here)
    └──────────┬───────────┘
               ▼
    ┌──────────────────────┐
    │ 3. Containment,      │
    │    Eradication &     │
    │    Recovery          │
    └──────────┬───────────┘
               ▼
    ┌──────────────────────┐
    │ 4. Post-Incident Act.│ (Lessons Learned & Reporting)
    └──────────────────────┘

```

1. **Preparation:** Policies, playbooks, communication channels, aur tools ko pehle se ready rakhna.
2. **Detection & Analysis:** Logs aur alerts analyze karke attack type, attacker IP, impacted machine aur attack timeline establish karna.
3. **Containment, Eradication & Recovery:**
* *Short-term Containment:* Machine ko network se isolate karna.
* *Eradication:* Malicious artifacts, registry persistence, aur drop hue files ko delete karna.
* *Recovery:* System ko clean backup se restore karke normal production mein wapas lana.

4. **Post-Incident Activity (Lessons Learned):** Post-mortem meeting karna: *"Attacker andar kaise aaya? Hamari detection kitni der baad trigger hui? Future mein ise kaise prevent karein?"*

---

## 7. SOC Operational Models

* **In-House SOC:** Company ki apni dedicated internal security team jo sirf usi enterprise ko monitor karti hai (High cost, maximum visibility).
* **MSSP (Managed Security Service Provider):** Outsourced SOC vendor jo ek sath 50-100 different client companies ke logs monitor karta hai (Multi-tenant environment).
* **Hybrid SOC:** Internal core team critical incidents handle karti hai jabki 24/7 overnight L1 triage MSSP vendor ko de diya jata hai.

---

## 8. High-Yield Interview Q&A (Day 1 Focus)

**Q1: What will you do if you receive a high-severity alert for a potential malware execution on an executive's laptop?**

* **Answer Approach:** First, verify the alert details in SIEM/EDR (Process ID, Parent Process, Hash reputation on VirusTotal). If confirmed malicious (True Positive), initiate immediate containment by network-isolating the endpoint via EDR to prevent lateral movement. Document all artifacts, notify the Incident Response lead/L2, and open an incident ticket according to the organization's SLA.

**Q2: What is the difference between an Indicator of Compromise (IOC) and an Indicator of Attack (IOA)?**

* **IOC (Forensic/Reactive):** Ye attack hone ke baad chhod gaye evidence hote hain (e.g., Known malicious IP, specific file hash, attacker domain).
* **IOA (Behavioral/Proactive):** Ye real-time attacker intent aur behavior ko darshata hai chahe file unknown ho (e.g., Word document suddenly launching PowerShell to execute Base64-encoded commands).

---

---

## Analyst takeaway / Yaad rakhne wali baat

Alert ≠ incident. Facts, hypotheses, severity and confidence ko alag rakho; escalation mein scope, evidence aur next action clear hona chahiye.

## Practice prompt / Khud try karo

Ek synthetic high-severity endpoint alert ka L1 handoff likho: observed facts, checks, confidence, escalation reason aur next owner.
