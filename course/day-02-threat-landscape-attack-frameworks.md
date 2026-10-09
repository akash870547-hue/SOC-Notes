# Day 02 — Cyber Threat Landscape, Lockheed Martin Cyber Kill Chain & MITRE ATT&CK Framework

> **What you'll learn / Is chapter mein:** Understand threat actors, the Cyber Kill Chain and ATT&CK as investigation frameworks.
>
> **Seedhi baat:** Attacker ka label guess karne se pehle uska observed behaviour dekho; frameworks ko investigation guide ki tarah use karo, proof ki tarah nahi.

**Course focus:** Threat Modeling, TTP Analysis & SOC Alert Mapping

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. The Modern Threat Landscape & Threat Actor Profiles

Cyber attacks random nahi hote; unke peeche specific motives, resources aur skillsets wale threat actors hote hain. SOC Analyst ko alert triage karte waqt attacker profile aur motive samajhna zaroori hota hai.

```
Threat Actor Spectrum:
[ Script Kiddies ] ──► [ Hacktivists ] ──► [ Cybercriminals / FIN Groups ] ──► [ Nation-State / APTs ]
(Low Skill / Tools)   (Ideological/PR)     (Financial / Ransomware / Extortion)    (Espionage / Critical Infra)

```

### Threat Actor Classification

* **Script Kiddies:**
* **English:** Unskilled individuals who use pre-written scripts, exploits, and automated scanning tools created by others, often without understanding how the underlying code functions.
* **Hinglish:** Ye low-skilled individuals hote hain jo internet par publicly available tools (jaise automated SQLi tools, DDoS scripts) download karke attack karte hain. Inka detection simple signature-based IDS/IPS rules se ho jata hai.

* **Hacktivists:**
* **English:** Politically or socially motivated attackers seeking to deface websites, leak sensitive data, or disrupt operations to draw attention to a cause.
* **Hinglish:** Inka motive paisa nahi balki ideological ya political protest hota hai (e.g., Anonymous group). Ye mostly Website Defacement, DDoS attacks, ya data leaks karte hain.

* **Cybercriminal Syndicates (e.g., FIN7, LockBit, BlackCat):**
* **English:** Highly organized, financially motivated syndicates specializing in ransomware-as-a-service (RaaS), banking trojans, and double-extortion schemes.
* **Hinglish:** Inka sole purpose financial gain hota hai. Ye professional enterprises ki tarah operate karte hain, specialized malware develop karte hain, aur corporate networks mein lateral movement karke business-critical data encrypt aur steal karte hain.

* **Nation-State Actors / APTs (Advanced Persistent Threats):**
* **English:** Government-sponsored, highly sophisticated groups with unlimited resources conducting stealthy, long-term cyber espionage, IP theft, or critical infrastructure sabotage (e.g., APT28/Fancy Bear, APT29/Cozy Bear, Lazarus Group).
* **Hinglish:** Inko foreign governments fund karti hain. Ye zero-day vulnerabilities use karte hain aur months/years tak corporate ya defense networks mein undetected chhupe rehte hain bina kisi noise ke data exfiltrate karne ke liye.

* **Insider Threats:**
* **English:** Disgruntled employees, negligent contractors, or compromised internal users with legitimate network credentials who abuse privileges or leak proprietary data.
* **Hinglish:** Ye sabse dangerous category hai kyunki inke paas authorized corporate credentials hote hain. Inhe detect karne ke liye User and Entity Behavior Analytics (UEBA) ki zaroorat padti hai.

---

## 2. Lockheed Martin Cyber Kill Chain

Lockheed Martin ne military concept ko adapt karke 7-stage linear framework banaya, jo batata hai ki ek traditional external cyber attack step-by-step kaise execute hota hai.

> **SOC Golden Rule:** Agar SOC team kisi bhi phase par attacker ki chain ko break kar deti hai, toh pura attack fail ho jata hai (Break the chain, defeat the attack).

```
[ 1. Reconnaissance ] ──► [ 2. Weaponization ] ──► [ 3. Delivery ] ──► [ 4. Exploitation ]
                                                                             │
[ 7. Actions on Objectives ] ◄── [ 6. Command & Control ] ◄── [ 5. Installation ]

```

### Phase-by-Phase Breakdown & SOC Detection Strategies

| Phase | Attacker Action | Attacker Tools / Techniques | SOC Telemetry & Detection |
| --- | --- | --- | --- |
| **1. Reconnaissance** | Target ke bare mein information gather karna (IPs, domain names, employee emails, open ports). | Shodan, WHOIS, Nmap, LinkedIn scraping, OSINT. | Port scans firewall/IDS logs mein track karna; honeypots hit hona; web server log access spikes. |
| **2. Weaponization** | Exploit ko payload (backdoor/malware) ke sath bundle karke malicious file banana. | Metasploit, MSFvenom, Macro builders, malicious PDFs. | Attacker ke private infrastructure par hota hai (No direct corporate telemetry). |
| **3. Delivery** | Weaponized payload ko target environment mein transmit karna. | Phishing emails with attachments, Malicious URLs, USB drops, Watering hole attacks. | Email Security Gateway (Proofpoint/Mimecast), Web Proxy logs, Firewall gateway AV alerts. |
| **4. Exploitation** | Target machine par vulnerability trigger karna ya user ko file open karne par convince karna. | MS Office macros, Buffer overflow, CVE exploits (e.g., Log4Shell, EternalBlue). | EDR telemetry, Antivirus alerts, Application crash logs, Windows Event ID 4688 (`winword.exe` launching `cmd.exe`). |
| **5. Installation** | Compromised host par backdoor ya web shell install karke persistent access banana. | Registry Run Keys, Scheduled Tasks, Windows Services, Startup folder drops. | Sysmon Event ID 1 (Process Create), Event ID 11 (File Create), Event ID 12/13 (Registry modification). |
| **6. Command & Control (C2)** | Victim system se attacker ke external control server ke beech communication channel open karna. | Reverse Shells, Cobalt Strike beaconing, DNS Tunneling, encrypted HTTPS sessions. | Proxy/DNS logs, unexpected outbound traffic on dynamic ports, periodic beaconing intervals (jitter analysis). |
| **7. Actions on Objectives** | Ultimate goal complete karna (Data theft, system encryption, server disruption). | Ransomware deployment, Mimikatz credential dumping, database dumping. | Mass file modifications (Ransomware behavior), Windows Event ID 4624/4672, high-volume outbound transfer. |

---

## 3. MITRE ATT&CK Framework

Lockheed Martin ka Kill Chain model linear (ek-tarfa) hai aur primarily perimeter-based defense par focus karta hai. Modern post-compromise threats (internal movement, privilege escalation) ko map karne ke liye industry standard ban chuka hai **MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge)**.

### Core Concepts: Tactic vs. Technique vs. Procedure (TTP)

* **Tactic (The 'Why' / Goal):** Attacker ka immediate objective kya hai? (e.g., *Initial Access*, *Persistence*, *Lateral Movement*).
* **Technique (The 'How' / Method):** Attacker us objective ko achieve karne ke liye kya tareeqa use kar raha hai? (e.g., Tactic = *Initial Access*; Technique = *T1566: Phishing*).
* **Sub-Technique:** Specific variant of a technique (e.g., *T1566.001: Spearphishing Attachment*).
* **Procedure (The Exact Execution):** Attacker ne specific tool/command kya run kiya? (e.g., APT29 ne malicious ISO image email ke through deliver kiya jisme hidden DLL side-loading executable tha).

### The 14 Enterprise Tactics (Logical Attack Flow)

```
[Reconnaissance] ──► [Resource Dev] ──► [Initial Access] ──► [Execution] ──► [Persistence]
       │
[Privilege Escalation] ──► [Defense Evasion] ──► [Credential Access] ──► [Discovery]
       │
[Lateral Movement] ──► [Collection] ──► [Command and Control] ──► [Exfiltration] ──► [Impact]

```

1. **Reconnaissance (TA0043):** Target context collect karna.
2. **Resource Development (TA0042):** Infrastructure (servers, domains, accounts) khareedna aur weapon ready karna.
3. **Initial Access (TA0001):** Network ke andar pehla pair jamana (Phishing, Exploit public-facing application).
4. **Execution (TA0002):** Malicious code run karna (PowerShell, Command-Line Interface, Scheduled Job).
5. **Persistence (TA0003):** System restart ya password change hone ke baad bhi access banaye rakhna (Registry autoruns, Account creation).
6. **Privilege Escalation (TA0004):** Standard user se Local Administrator ya Domain Admin banna.
7. **Defense Evasion (TA0005):** Antivirus/EDR ko bypass ya disable karna, security logs clear karna (Windows Event 1102).
8. **Credential Access (TA0006):** Passwords, hashes, aur Kerberos tickets churna (LSASS memory dump via Mimikatz - T1003).
9. **Discovery (TA0007):** Internal environment ko explore karna (*"Where am I? What else is on this network?"* via `whoami`, `net user`, `net view`).
10. **Lateral Movement (TA0008):** Ek compromised machine se doosre servers par jump karna (RDP, PsExec, Pass-the-Hash - T1550).
11. **Collection (TA0009):** Files, emails, aur sensitive documents ko ek jagah gather karna (Archiving via 7-Zip).
12. **Command and Control (TA0011):** External C2 server ke sath connection maintain karna (Encrypted web protocols, DNS tunneling).
13. **Exfiltration (TA0010):** Gather kiya gaya data company network se bahar bhejna (Exfiltration over C2, cloud storage uploads).
14. **Impact (TA0040):** Business operations ko destroy karna (Ransomware encryption, Disk wipe).

---

## 4. Cyber Kill Chain vs. MITRE ATT&CK Matrix

| Parameter | Cyber Kill Chain (Lockheed Martin) | MITRE ATT&CK Framework |
| --- | --- | --- |
| **Structure** | Strict linear, step-by-step 7-phase model. | Non-linear, granular matrix (14 Tactics, 190+ Techniques). |
| **Focus Area** | Perimeter defense & initial intrusion prevention. | Post-compromise internal adversary behavior. |
| **Flexibility** | Rigid: Assumes attack follows 1 to 7 sequence. | Flexible: Attacker kisi bhi stage se jump kar sakta hai aur techniques repeat kar sakta hai. |
| **SOC Usage** | High-level executive reporting aur incident classification. | Detection engineering, SIEM correlation rules, threat hunting, gap analysis. |

---

## 5. End-to-End SOC Real-World Attack Scenario

```
[Phishing Mail Received] ──► [User Opens Word File] ──► [PowerShell Spawns] ──► [C2 Beacon Outbound]
 (Initial Access / T1566)     (Execution / T1204)       (Execution / T1059)      (C2 / T1071.001)
                                                                 │
[Data Stolen / Impact] ◄── [RDP to File Server] ◄── [Mimikatz LSASS Dump] ◄──────┘
 (Exfiltration / T1048)     (Lateral Move / T1021)    (Cred Access / T1003)

```

1. **Step 1:** Finance team member ko email aati hai: *"Invoice_Q3.docx"* attached (Phishing: T1566.001).
2. **Step 2:** User file open karta hai; VBA macro background mein chalti hai (User Execution: T1204.002).
3. **Step 3:** Macro background mein `powershell.exe -enc ...` execute karti hai (Command & Scripting Interpreter: T1059.001).
4. **Step 4:** PowerShell external IP (`185.220.101.5:443`) par HTTPS beacon connect karti hai (Application Layer Protocol: T1071.001).
5. **Step 5:** Attacker LSASS memory read karke Domain Admin ke credentials nikaalta hai (OS Credential Dumping: T1003.001).
6. **Step 6:** Attacker un credentials se Core File Server par RDP karta hai (Remote Desktop Protocol: T1021.001).
7. **Step 7:** Critical files encrypt karke `.locked` extension laga deta hai (Data Encrypted for Impact: T1486).

---

## 6. High-Yield Interview Q&A (Day 2 Focus)

**Q1: What is the primary operational limitation of the Lockheed Martin Cyber Kill Chain?**

* **Answer:** Cyber Kill Chain assume karta hai ki har attack perimeter se shuru hota hai aur sequentially 7 steps follow karta hai. Ye modern scenarios ko effectively address nahi karta, jaise: insider threats (jahan delivery/exploitation ki zaroorat nahi hoti), cloud identity compromise, aur post-exploitation lateral movement. Is limitation ko solve karne ke liye SOC teams MITRE ATT&CK use karti hain jo non-linear post-compromise behavior par focus karta hai.

**Q2: How does a SOC use the MITRE ATT&CK framework for 'Gap Analysis'?**

* **Answer:** SOC team apne existing detection tools (SIEM rules, EDR alert signatures) ko MITRE ATT&CK matrix par map karti hai. Jo techniques unke tools detect nahi kar paate (e.g., Matrix par red/blank blocks), unhe 'Gaps' ke roop mein identify kiya jata hai. Phir Detection Engineers un specific techniques (e.g., T1055 Process Injection) ke liye new logging aur correlation rules build karte hain.

---

---

---

## Analyst takeaway / Yaad rakhne wali baat

ATT&CK aur Kill Chain investigation ko structure karte hain. Observed behaviour support kare tabhi technique mapping ya attribution likho.

## Practice prompt / Khud try karo

Ek hypothetical phishing → execution → persistence timeline banao aur har phase ke liye likely telemetry source list karo.
