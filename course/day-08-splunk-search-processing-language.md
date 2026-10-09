# Day 08 — Splunk Search Processing Language (SPL) & Querying for SOC Threat Hunting

> **What you'll learn / Is chapter mein:** Write SPL searches that filter, aggregate and correlate evidence without losing important context.
>
> **Seedhi baat:** Query ka result hi poori investigation nahi. Aggregate se signal nikalo, phir raw events par laut kar reasoning validate karo.

**Course focus:** Writing Efficient SOC Queries, Data Filtering, Statistical Aggregations & Threat Correlation

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. Anatomy of an SPL Search Pipeline

Splunk ka core search engine UNIX pipeline philosophy par kaam karta hai. Data left-to-right travel karta hai aur pipe character (`|`) se ek command ka output agle command ka input ban jata hai.

```
[ Search Head: Index Filter ] ──► | [ Transform / Filter ] ──► | [ Calculate / Stats ] ──► | [ Format / Display ]
index=windows EventCode=4625       | where count > 5            | stats count by user        | sort - count

```

### The 3 Rules of Writing Performant SPL

1. **Filter Early (Search Left of the First Pipe):** First pipe aane se pehle jitna ho sake data filter kar lo (`index`, `sourcetype`, `Earliest/Latest` time window, specific fields).
2. **Limit Raw Data Extraction:** `*` wildcard ko search ke starting word mein use mat karo (e.g., `*error` indexer ko brute search karne par force karta hai, jabki `error*` index file use karta hai).
3. **Drop Unnecessary Fields:** Search stream mein jaldi se `fields` command use karke unused metadata fields ko discard karo memory overhead kam karne ke liye.

---

## 2. Essential SPL Commands for SOC Analysts

### A. Display & Field Formatting Commands

* **`fields`:** Sirf specified fields ko memory mein retain karta hai ya remove karta hai.
```spl
index=windows sourcetype=WinEventLog:Security EventCode=4624
| fields _time, user, src_ip, Logon_Type

```

* **`table`:** Output ko neat tabular format mein convert karta hai.
```spl
index=firewall action=blocked
| table _time, src, src_port, dest, dest_port, rule_name

```

* **`rename`:** Field names ko SOC presentation ke liye user-friendly banata hai.
```spl
| rename src_ip as "Attacker_IP", count as "Failed_Attempts"

```

### B. Statistical Aggregation Commands (`stats`)

SOC investigations mein raw events dekhne se patterns samajh nahi aate; `stats` command data ko aggregate karke baseline deviations reveal karti hai.

| Stats Function | Description | SOC Use Case |
| --- | --- | --- |
| `count` | Total number of occurrences. | Failed login counts, firewall drop frequency. |
| `distinct_count` / `dc` | Unique values ka count nikaalna. | Ek IP se kitne distinct usernames attack hue (Password Spraying). |
| `values(field)` | Unique values ki clean list print karna. | Ek user ne kitne unique machines par login try kiya. |
| `list(field)` | Saare occurrences raw chronological order mein print karna. | Process execution sequence analyze karna. |
| `sum(field)` | Numeric values ka sum nikaalna. | Outbound data exfiltration bytes calculate karna. |

```spl
`stats` Command Example for Port Scan Detection:
index=firewall action=blocked
| stats count, dc(dest_port) as unique_ports by src
| where unique_ports > 100
| sort - unique_ports

```

### C. Filtering & Evaluation Commands (`where`, `eval`)

* **`eval`:** Naye dynamic fields create karna, string concatenation karna, ya mathematical logic implement karna.
```spl
index=proxy
| eval data_mb = round(bytes_out / (1024 * 1024), 2)
| where data_mb > 500

```

* **Conditional Logic with `case` / `if`:**
```spl
index=windows sourcetype=WinEventLog:Security EventCode=4624
| eval Logon_Category = case(
    Logon_Type==2, "Interactive_Console",
    Logon_Type==3, "Network_Share",
    Logon_Type==10, "Remote_Desktop_RDP",
    1=1, "Other_Type")
| stats count by Logon_Category

```

### D. Regular Expression Field Extraction (`rex`)

Jab raw log unparsed hota hai aur vendor field automatically extract nahi hoti, tab `rex` use kiya jata hai.

```spl
Raw Log Sample: "User admin from IP 192.168.1.105 failed authentication"

index=auth_logs
| rex field=_raw "User\s(?<target_user>\w+)\sfrom\sIP\s(?<source_ip>\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"
| table _time, target_user, source_ip

```

---

## 3. Threat Hunting & SOC Practical SPL Use Cases

### Use Case 1: Detecting RDP Brute Force Attacks

**Objective:** Aise external IPs identify karna jinhone specific machine par 10 minute ke window mein 10 se zyada failed logins (Event 4625) kiye aur RDP (Logon Type 10) target kiya.

```spl
index=windows sourcetype=WinEventLog:Security EventCode=4625 Logon_Type=10 earliest=-24h
| stats count as failed_attempts, values(user) as targeted_users by src_ip, dest
| where failed_attempts >= 10
| sort - failed_attempts

```

### Use Case 2: Detecting Password Spraying Campaigns

**Objective:** Single IP address multiple distinct user accounts par login try kar raha hai (Broad attack with low per-account count to evade lockout policies).

```spl
index=windows sourcetype=WinEventLog:Security EventCode=4625 earliest=-1h
| stats dc(user) as unique_targeted_accounts, count as total_failed_logins by src_ip
| where unique_targeted_accounts > 15
| sort - unique_targeted_accounts

```

### Use Case 3: Outbound Data Exfiltration Anomaly Hunt

**Objective:** Kaunsa internal asset corporate perimeter ke bahar abnormal volume of data upload kar raha hai.

```spl
index=firewall direction=outbound earliest=-24h
| eval transferred_mb = round(bytes_sent / 1048576, 2)
| stats sum(transferred_mb) as total_uploaded_mb, values(dest_ip) as contacted_ips by src_ip
| where total_uploaded_mb > 1024
| sort - total_uploaded_mb

```

### Use Case 4: C2 Beaconing Jitter Analysis using `timechart`

**Objective:** Identify regular periodic connections to an external IP (automated heartbeat/beacon).

```spl
index=proxy dest_ip="185.220.101.5" earliest=-6h
| timechart span=1m count by src_ip

```

*Agar timechart graph ek straight horizontal flat line dikhata hai (har 1 minute mein exactly 1 ya 2 requests bina human variation ke), toh ye high-confidence Command & Control (C2) beacon hai.*

---

## 4. `transaction` vs. `stats` (Performance Comparison)

Interviewers frequently evaluate an analyst's architectural maturity with this comparison:

```
[ Method 1: stats ]        ──► Fast, Memory-Efficient, Distributed Processing across Indexers
[ Method 2: transaction ]  ──► Slow, High Memory Overhead, Single Search Head Bottleneck

```

| Parameter | `stats` Command | `transaction` Command |
| --- | --- | --- |
| **Execution Location** | Distributed: Calculation Indexer level par execute hoti hai. | Centralized: Saara raw data Search Head par pull hota hai processing ke liye. |
| **Performance** | Extremely Fast; scalable across millions of logs. | Resource-intensive; large datasets par timeout ho sakta hai. |
| **When to Use** | Aggregations, counts, calculations, distinct values. | Tab use karein jab session start and end events track karni ho (e.g., Session Duration, Max Pause between steps). |

---

## 5. High-Yield Interview Q&A (Day 8 Focus)

**Q1: How do you optimize a slow-running Splunk query in a production SOC environment?**

* **Answer:**
1. Specify explicit `index` and `sourcetype` instead of wildcards.
2. Tighten the time boundary (`earliest` and `latest`) rather than searching `All time`.
3. Place all static text filters and exclusions before the first pipe (`|`).
4. Use the `fields` command immediately after the initial search to drop unnecessary metadata fields from memory.
5. Replace the `transaction` command with `stats` wherever session-tracking conditions permit.

**Q2: Write an SPL search to find all instances where an account had multiple failed logins followed by a successful login from the same IP within 1 hour.**

* **Answer:**
```spl
index=windows sourcetype=WinEventLog:Security (EventCode=4625 OR EventCode=4624) earliest=-1h
| stats count(eval(EventCode=4625)) as Failed_Logins,
        count(eval(EventCode=4624)) as Successful_Logins,
        min(_time) as First_Attempt,
        max(_time) as Last_Attempt
        by src_ip, user
| where Failed_Logins >= 5 AND Successful_Logins >= 1
| eval Window_Duration_Minutes = round((Last_Attempt - First_Attempt)/60, 2)
| table src_ip, user, Failed_Logins, Successful_Logins, Window_Duration_Minutes

```

---

## Analyst takeaway / Yaad rakhne wali baat

Readable, bounded aur reproducible queries likho; thresholds environment-specific hote hain.

## Practice prompt / Khud try karo

Do SPL searches likho: ek summary/aggregation aur ek raw-event validation query; dono ka scope note karo.
