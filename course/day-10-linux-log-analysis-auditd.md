# Day 10 — Linux Log Analysis, Auditd Engine & Incident Threat Hunting

> **What you'll learn / Is chapter mein:** Use Linux authentication/system logs and audit telemetry to build a defensible timeline.
>
> **Seedhi baat:** Ek failed SSH login noise bhi ho sakta hai, attack bhi. Frequency, source, account, successful follow-up aur host context se decision banta hai.

**Course focus:** Analyzing `/var/log`, Detecting SSH Brute Force, Web Shell Execution & Tamper Detection

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. Linux Logging Architecture: Key Log Files

Linux system par core security events `/var/log/` directory ke andar plaintext ya compressed format mein record hote hain:

```
/var/log/
├── auth.log  (Debian / Ubuntu)   ──► Authentication, Logins, Sudo privileges
├── secure    (RHEL / CentOS)     ──► Authentication equivalent on RedHat systems
├── syslog / messages             ──► General system, kernel & service messages
├── audit/audit.log               ──► Linux Audit Daemon (System calls, file access)
├── wtmp & utmp                   ──► Successful user login sessions (read via `last`)
└── btmp                          ──► Failed login history (read via `lastb`)

```

### Log File Breakdown for SOC Analysts

| Log File | Linux Distribution | Information Recorded | Critical SOC Use Case |
| --- | --- | --- | --- |
| **`/var/log/auth.log`** | Ubuntu, Debian, Kali | SSH Logins, PAM authentications, `sudo` commands executed. | SSH Brute force, unauthorized root escalations. |
| **`/var/log/secure`** | RHEL, CentOS, Rocky, Fedora | RedHat equivalent of auth.log. | Authentication tracking on Enterprise Linux. |
| **`/var/log/audit/audit.log`** | All distros running `auditd` | Granular kernel-level system call executions (`execve`). | Detecting execution of commands even if logs are modified. |
| **`~/.bash_history`** | User home directories | Commands executed by users in interactive bash shells. | Analyzing attacker activity post-compromise. |

---

## 2. Analyzing Authentication Logs: SSH Brute Force Anatomy

### Anatomy of a Failed SSH Login Log (Brute Force Indicator)

```syslog
Oct 9 11:20:15 server01 sshd[28412]: Failed password for invalid user admin from 198.51.100.24 port 54122 ssh2
Oct 9 11:20:18 server01 sshd[28415]: Failed password for invalid user test from 198.51.100.24 port 54130 ssh2
Oct 9 11:20:21 server01 sshd[28419]: Failed password for root from 198.51.100.24 port 54138 ssh2

```

* **Key Indicators:** Same Source IP (`198.51.100.24`), short time gaps (3-second intervals), rotating common usernames (`admin`, `test`, `root`), changing source ports.

### Anatomy of a Successful SSH Login Log

```syslog
Oct 9 11:25:40 server01 sshd[28550]: Accepted password for deployer from 198.51.100.24 port 54290 ssh2
Oct 9 11:25:40 server01 sshd[28550]: pam_unix(sshd:session): session opened for user deployer by (uid=0)

```

* **Critical Correlation:** Multiple `Failed password` events ke turant baad agar same IP se `Accepted password` log generate hota hai, toh ye **Successful Brute-Force Breach** confirm karta hai.

---

## 3. Investigating Privilege Escalation (`sudo` Logs)

Jab koi compromised standard account root privileges execute karta hai:

```syslog
Oct 9 11:30:12 server01 sudo:  deployer : TTY=pts/0 ; PWD=/home/deployer ; USER=root ; COMMAND=/bin/su -
Oct 9 11:30:12 server01 sudo: pam_unix(sudo:session): session opened for user root by deployer(uid=1001)

```

* **Fields Analyzed by SOC Analyst:**
* `deployer`: Legitimate user who issued the command.
* `USER=root`: Target execution privilege (user attempted to escalate to root).
* `COMMAND=/bin/su -`: The exact binary invoked (user switched completely to the root user environment).

---

## 4. Hunting via Command-Line Log Parsing (Bash Pipeline)

SOC analysts Linux systems par large log files ko fast inspect karne ke liye built-in CLI tools use karte hain:

### Hunt 1: Top 10 IP Addresses Attempting SSH Brute Force

```bash
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -n 10

```

* **Command Breakdown:**
1. `grep "Failed password"`: Sirf failed logins isolate karta hai.
2. `awk '{print $(NF-3)}'`: Log line ke end se fourth field (jo ki Source IP hota hai) extract karta hai.
3. `sort | uniq -c`: Har unique IP ke fail counts calculate karta hai.
4. `sort -nr`: Descending order mein sort karta hai (highest attacker counts on top).

### Hunt 2: Identifying User Accounts Targeted by Attackers

```bash
grep "Failed password for invalid user" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

```

---

## 5. Linux Audit Daemon (`auditd`) & System Call Tracking

Standard syslog files easily tamper ho sakti hain agar attacker root access pa leta hai. `auditd` kernel level par baithta hai aur system calls capture karta hai.

* **Auditing Rule Configuration (`/etc/audit/rules.d/audit.rules`):**
```bash
# Watch for any modification to /etc/passwd or /etc/shadow
-w /etc/passwd -p wa -k identity_tampering
-w /etc/shadow -p wa -k identity_tampering

```

*(Permissions: `w` = Write, `a` = Attribute change, `-k` = Custom search key).*
* **Querying Audit Logs with `ausearch`:**
```bash
ausearch -k identity_tampering --format text

```

---

## 6. Detecting Anti-Forensics & Log Tampering in Linux

Attacker system chhodne se pehle logs clear karne ki koshish karta hai:

```
[ Common Anti-Forensics Techniques in Linux ]
├── 1. Log Truncation (> /var/log/auth.log  or  rm -rf /var/log/*)
├── 2. Disabling Shell History (unset HISTFILE  or  export HISTSIZE=0)
├── 3. Space-Prefixed Command Execution (Commands starting with space evade history)
└── 4. Timestomping (touch -r legitimate_file.txt malware.sh)

```

* **SOC Detection:**
* SIEM mein alert rule: *“Log Ingestion Stopped / Heartbeat Missing from Linux Server”*.
* Auditd alert on system calls executing `rm` inside `/var/log`.
* `~/.bash_history` file ka suddenly zero bytes size ho jana ya `/dev/null` par symlink hona.

---

## 7. High-Yield Interview Q&A (Day 10 Focus)

**Q1: How do you detect if an attacker has cleared or altered the bash command history?**

* **Answer:**
1. Check the file size and permissions of `~/.bash_history`. If it is 0 bytes or redirected to `/dev/null` (`ls -l ~/.bash_history -> /dev/null`), tampering has occurred.
2. Check current environment variables using `env` or `echo $HISTFILE $HISTSIZE`. If `HISTSIZE=0` or `HISTFILE` is unset, logging is intentionally disabled.
3. Check the process tree or `/var/log/audit/audit.log` for commands like `history -c`, `unset HISTFILE`, or `shred -u ~/.bash_history`.

---

---

---

## Analyst takeaway / Yaad rakhne wali baat

Negative hunt result ka matlab sirf itna hai ki defined scope/data mein evidence nahi mila—not proof that activity never happened.

## Practice prompt / Khud try karo

SSH brute-force hunt ke liye hypothesis, time window, fields, threshold rationale aur telemetry gaps likho.
