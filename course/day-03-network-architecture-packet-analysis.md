# Day 03 — Network Architecture, OSI vs. TCP/IP Packet Flow & Deep Packet Analysis for SOC

> **What you'll learn / Is chapter mein:** Build a mental model of network flow, packet analysis and the evidence Wireshark can expose.
>
> **Seedhi baat:** Packet capture mein har packet story ka ek tukda hai. Direction, protocol, timestamps aur surrounding traffic ko saath mein samajhna zaroori hai.

**Course focus:** Traffic Triage, Wireshark Packet Hunting & Protocol Anomalies

> **Study note:** Core content is based on the supplied text notes. Formatting and explanatory context have been edited for readability. Commands, thresholds and detection examples are illustrative; validate them in an authorized lab and against your own data schema before operational use.

---

## 1. OSI 7-Layer Model vs. TCP/IP Model (SOC Investigation Lens)

Networking concepts ko sirf exam pass karne ke liye nahi, balki packet level par attacker payload aur communication channel identify karne ke liye samjha jata hai.

```
OSI 7-Layer Model                     TCP/IP 4-Layer Model        SOC Investigation Artifacts
┌─────────────────────────┐          ┌─────────────────────┐    ┌─────────────────────────────────┐
│ 7. Application Layer    │ ──┐      │                     │    │ HTTP, DNS, SMTP, TLS, User-Agent│
│ 6. Presentation Layer   │ ──┼────► │  Application Layer  │ ──►│ Payloads, SSL Certificates      │
│ 5. Session Layer        │ ──┘      │                     │    │ Session IDs, RPC calls          │
├─────────────────────────┤          ├─────────────────────┤    ├─────────────────────────────────┤
│ 4. Transport Layer      │ ───────► │  Transport Layer    │ ──►│ TCP/UDP Ports, Flags (SYN/ACK/RST)
├─────────────────────────┤          ├─────────────────────┤    ├─────────────────────────────────┤
│ 3. Network Layer        │ ───────► │  Internet Layer     │ ──►│ Source/Destination IP, TTL, ICMP│
├─────────────────────────┤          ├─────────────────────┤    ├─────────────────────────────────┤
│ 2. Data Link Layer      │ ──┐      │  Network Access     │ ──►│ MAC Addresses, ARP, VLAN Tags   │
│ 1. Physical Layer       │ ──┴────► │  (Link) Layer       │ ──►│ Cables, Physical NIC, Signals   │
└─────────────────────────┘          └─────────────────────┘    └─────────────────────────────────┘

```

### Layer-by-Layer SOC Threat Mapping

* **Layer 7 (Application):** Yahan web application attacks (SQL Injection, XSS, Path Traversal), DNS tunneling queries, aur Phishing email headers detect hote hain.
* **Layer 4 (Transport):** Yahan port scans (Nmap SYN scan), unauthorized listening ports, aur TCP connection state anomalies (e.g., high volume SYN packets bina ACK ke = SYN flood) analyze hote hain.
* **Layer 3 (Network / Internet):** Source and Destination IP tracking, GeoIP analysis, IP spoofing, aur TTL-based OS fingerprinting yahan perform hoti hai.
* **Layer 2 (Data Link):** Internal network attacks jaise ARP Spoofing/Poisoning, Rogue DHCP servers, aur MAC flooding yahan pakde jaate hain.

---

## 2. Data Encapsulation & Decapsulation

Jab koi system internet par data bhejta hai, toh top-to-bottom har layer apna metadata add karti hai (Encapsulation). Receiving end par bottom-to-top ye headers strip-off hote hain (Decapsulation).

```
[ Application Data ]                                              (Data)
        │
        ▼  Add TCP Header (Source Port, Dest Port, Seq No)
[ TCP Header | Application Data ]                                 (Segment)
        │
        ▼  Add IP Header (Source IP, Dest IP, TTL)
[ IP Header | TCP Header | Application Data ]                     (Packet)
        │
        ▼  Add Ethernet Header & Trailer (Source MAC, Dest MAC, CRC)
[ Eth Header | IP Header | TCP Header | Application Data | Frame Check ]  (Frame)
        │
        ▼  Physical Transmission
01101001011000110110100101100011                                  (Bits)

```

---

## 3. End-to-End Packet Flow Analysis (Step-by-Step Scenario)

**Scenario:** Client machine (`192.168.1.50`) browser mein open karti hai `[https://malicious-c2.com](https://malicious-c2.com)`. Packet network mein actually kaise travel karta hai?

```
[ Client: 192.168.1.50 ]
        │
        ├── Step 1: DNS Query (UDP 53) ──► Resolves 'malicious-c2.com' to 203.0.113.15
        │
        ├── Step 2: ARP Request (Broadcast) ──► Finds MAC of Default Gateway (Router)
        │
        ├── Step 3: TCP 3-Way Handshake ──► SYN ──► SYN-ACK ──► ACK to 203.0.113.15:443
        │
        └── Step 4: TLS 1.3 Handshake ──► Client Hello ──► Server Hello ──► Encrypted Data

```

1. **DNS Resolution (UDP 53):** Client system ko domain name ka IP address chahiye. Wo local configured DNS resolver ko query bhejta hai. DNS resolver return karta hai IP: `203.0.113.15`.
2. **Routing Decision & ARP (Address Resolution Protocol):** Client check karta hai: *“Kya 203.0.113.15 mere local subnet (192.168.1.0/24) mein hai?”* Nahi. Iska matlab traffic Default Gateway (Router) ko bhejna padega.
* Client broadcast karta hai: *"Who has 192.168.1.1? Tell 192.168.1.50"* (ARP Request).
* Router reply karta hai: *"192.168.1.1 is at 00:1A:2B:3C:4D:5E"* (ARP Reply).

3. **TCP 3-Way Handshake (Port 443):**
* **SYN:** Client bhejta hai Synchronize packet server IP `203.0.113.15` ko with initial sequence number.
* **SYN-ACK:** Server acknowledge karta hai aur apna sync number bhejta hai.
* **ACK:** Client acknowledge karta hai. Connection ESTABLISHED ho jata hai.

4. **TLS Encrypted Handshake:** Client aur Server cryptographic keys negotiate karte hain. Once completed, application data fully encrypted format mein travel karta hai.

---

## 4. Deep Packet Inspection & Wireshark Threat Hunting

SOC Analysts Wireshark ya Zeek (Bro) use karke PCAP (Packet Capture) files inspect karte hain jab alert ko packet level par verify karna hota hai.

```
Wireshark Investigation Checklist:
[ Protocol Hierarchy ] ──► [ Conversations (IP/TCP) ] ──► [ Follow TCP Stream ] ──► [ Export Objects ]
(Broad Volume Check)       (Top Talkers & Data Size)      (Reconstruct Session)     (Extract Dropped Files)

```

### Essential Wireshark Display Filters for Incident Analysis

| Filter Syntax | Investigation Purpose |
| --- | --- |
| `ip.addr == 192.168.1.50` | Specific suspected host ka sara inbound aur outbound traffic isolate karna. |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Port scans ya SYN flood attacks identify karna (sirf initiation packets). |
| `dns.flags.response == 0` | All outgoing DNS queries dekhna (DNS Tunneling ya DGA domains hunting). |
| `http.request.method == "POST"` | Attacker ko data upload ya credential submission trace karna. |
| `frame contains "password" |  |
| `tcp.port == 4444 |  |

### The "Follow TCP Stream" Technique

Wireshark mein kisi bhi packet par Right-click ➔ **Follow ➔ TCP Stream** karne se raw packets reconstruct hokar human-readable conversation ban jaate hain.

* Agar protocol unencrypted hai (HTTP/Telnet/FTP), toh attacker dwara run ki gayi commands (`whoami`, `cat /etc/passwd`) directly red aur blue text mein dikh jaati hain.

---

## 5. Critical Network Layer Attack Patterns

### 1. SYN Flood Attack (Denial of Service)

* **Mechanics:** Attacker hazaron SYN packets bhejta hai spoofed IPs se, lekin server ke SYN-ACK ka kabhi ACK return nahi karta.
* **Impact:** Server ki memory mein connection queue (backlog buffer) full ho jati hai, aur legitimate users ke liye server unresponsive ho jata hai.
* **SOC Detection:** Wireshark/SIEM mein abnormally high ratio of SYN packets compared to ESTABLISHED sessions.

### 2. ARP Spoofing / Poisoning (Man-in-the-Middle)

* **Mechanics:** Attacker internal network mein fake gratuitous ARP replies broadcast karta hai claiming ki Default Gateway ki IP attacker ke MAC address par mapped hai.
* **Impact:** Sara internal traffic pehle attacker ki machine se pass hota hai, jahan wo packets sniff ya alter kar sakta hai.
* **SOC Detection:** Ek hi IP address multiple MAC addresses se associate hote hue dikhna, ya switch par Dynamic ARP Inspection (DAI) alert trigger hona.

### 3. Data Exfiltration via Network Covert Channels

* **Mechanics:** Standard file transfer ports block hone par attacker ICMP Echo Request (Ping) packets ke data field mein sensitive data encrypt karke external server par bhejta hai.
* **SOC Detection:** Normal ICMP packets standard 32-64 bytes ke hote hain. Agar PCAP mein ICMP packets 500-1500 bytes ke dikhein, toh ye immediate data exfiltration ka indicator hai.

---

## 6. High-Yield Interview Q&A (Day 3 Focus)

**Q1: What is the difference between TCP SYN Scan (Stealth Scan) and TCP Connect Scan in Nmap?**

* **Answer:**
* **TCP Connect Scan (`-sT`):** Full 3-way handshake (SYN, SYN-ACK, ACK) complete karta hai. Ye OS system calls use karta hai aur target application ke connection logs mein easily record ho jata hai.
* **TCP SYN Scan (`-sS`):** Half-open scan hai. Ye target se SYN-ACK milte hi connection complete karne ki jagah turant RST (Reset) packet bhej kar connection break kar deta hai. Full connection establish na hone ki wajah se application layer par log record nahi hota, isliye ise stealth scan kaha jata hai (halaanki modern firewalls aur IDS ise easily detect kar lete hain).

**Q2: If an attacker is using HTTPS (port 443) for Command and Control communication, how can a SOC analyst detect it if the payload is fully encrypted?**

* **Answer:** Even without SSL/TLS decryption, network metadata se attacker pakda ja sakta hai:
1. **Beaconing Jitter & Timing:** Automated bots har fixed interval (e.g., exactly every 60 seconds) par heartbeat request bhejte hain.
2. **JA3 / JA3S Fingerprinting:** Client Hello packet ke SSL parameters (ciphers, extensions) ka cryptographic hash calculate karke known malware families (Cobalt Strike, TrickBot) se match karna.
3. **SNI (Server Name Indication) & Certificate Inspection:** Check karna ki destination certificate self-signed hai ya untrusted/newly created domain par host hai.
4. **Data Transfer Ratio:** Request aur response size ke ratio mein asymmetry analyze karna.

---

---

## Analyst takeaway / Yaad rakhne wali baat

PCAP analysis ko bounded time range se start karo, stream/direction correlate karo aur missing visibility note karo.

## Practice prompt / Khud try karo

A synthetic PCAP exercise ke liye source/destination, DNS, HTTP/TLS metadata aur timezone ko record karne ki checklist banao.
