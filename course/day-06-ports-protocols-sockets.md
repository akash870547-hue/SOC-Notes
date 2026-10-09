# Day 06 — Ports, Protocols & Sockets

> **Source status:** The supplied text contains Day 6 in the curriculum table, but not a detailed Day 6 chapter. This chapter is an editorial expansion of that outline—not a transcription of missing notes.

> **What you'll learn / Is chapter mein:** Understand TCP vs UDP, sockets, common service ports, DNS tunnelling signals and DHCP starvation from a defender's point of view.
>
> **Seedhi baat:** Port number bas ek clue hai, poora proof nahi. SOC analyst ko connection ka context, direction, frequency aur endpoint behaviour samajhna hota hai.

## 1. IP, protocol, port and socket — the basic picture

- **IP address:** identifies a network interface / endpoint in a given network context.
- **Transport protocol:** TCP or UDP is the transport layer behaviour used by an application.
- **Port:** identifies a service endpoint within a host's transport namespace.
- **Socket:** a communication endpoint; a network flow is often described by source IP/port, destination IP/port and protocol (a 5-tuple).

Example: `10.10.4.12:51544 → 10.10.8.20:443 / TCP` means a client ephemeral port is communicating with a destination's TCP port 443. It does not by itself prove the app is HTTPS or that the traffic is safe.

## 2. TCP vs UDP

| Property | TCP | UDP |
|---|---|---|
| Connection model | Connection-oriented | Connectionless datagrams |
| Delivery | Reliable, ordered byte stream | No built-in delivery or ordering guarantee |
| Handshake | Commonly uses SYN → SYN/ACK → ACK | No TCP-style handshake |
| Overhead | More protocol state and control | Lower protocol overhead |
| SOC examples | HTTPS, SSH, RDP, SMB | DNS, NTP, DHCP; some voice/video |
| Useful telemetry | Handshake failures, resets, retransmissions, session duration | Request/response patterns, bursts, rate, payload metadata where visible |

TCP three-way handshake (simplified):

```text
Client                         Server
  | -------- SYN ------------->  |
  | <----- SYN + ACK ---------- |
  | -------- ACK ------------->  |
        Connection established
```

Connection setup is normal behaviour. A large volume of SYNs without completion may indicate scanning or resource exhaustion, but configuration, load balancers and packet loss can produce similar observations.

## 3. Common ports — quick analyst reference

| Port / Protocol | Common use | What to validate |
|---|---|---|
| TCP 20/21 | FTP data/control | Cleartext credential/data risk, expected server role |
| TCP 22 | SSH | Approved source, account, authentication outcome |
| TCP 23 | Telnet | Legacy exposure; plaintext management risk |
| TCP 25 / 587 / 465 | SMTP / submission variants | Mail flow and relay policy |
| TCP 53 / UDP 53 | DNS | Query/response patterns; TCP is also used for large responses/zone transfer contexts |
| UDP 67/68 | DHCP server/client | Lease negotiation, rogue server or abnormal request rate |
| TCP 80 | HTTP | Host, method, URI, status, proxy visibility |
| UDP 123 | NTP (commonly UDP) | Time-source consistency and clock drift |
| TCP 135 | Windows RPC endpoint mapper | Expected Windows management behaviour |
| TCP 139 / 445 | NetBIOS session / SMB | File sharing, remote admin, lateral movement context |
| TCP 389 / 636 | LDAP / LDAPS | Directory traffic, secure transport and source roles |
| TCP 443 | HTTPS | TLS metadata, SNI/HTTP fields where available, endpoint process |
| TCP 3389 | RDP | Source, account, logon type, approved remote access |
| TCP 3306 / 5432 / 1433 | Common database ports | Database exposure and expected app-to-DB paths |

Ports can be changed, proxied or reused. Protocol identification must be based on actual telemetry, not port numbers alone.

## 4. DNS tunnelling — signals, not instant verdicts

DNS tunnelling can encode data or commands inside DNS queries/responses. A SOC hunt may look for:
- unusually long or high-entropy labels;
- high query volume to one domain or many frequently changing subdomains;
- rare domains and repetitive query patterns;
- abnormal TXT / NULL or other record-type usage relative to the environment;
- one process making DNS requests at unusual intervals;
- supporting endpoint or proxy activity that makes the hypothesis stronger.

### Example investigation flow

```text
DNS alert
   ↓
Confirm resolver, client, domain, record type and time window
   ↓
Compare with baseline + other clients
   ↓
Pivot to endpoint process and outbound connections
   ↓
Check benign explanations (agents, CDN, update service, telemetry)
   ↓
Document confirmed signal, uncertainty and next action
```

A long DNS label is not enough to call tunnelling. Use evidence from multiple sources and preserve the query, response, and context.

## 5. DHCP starvation / rogue DHCP awareness

- **DHCP starvation:** a denial-of-service pattern where a DHCP service is overwhelmed by an abnormal volume of lease requests / client identifiers.
- **Rogue DHCP server:** an unauthorized server supplies address, gateway or DNS configuration, potentially redirecting clients.

Defensive telemetry may include DHCP server logs, switch security features, access-control logs, unusual request volumes, many client MAC addresses, multiple DHCP offers, lease exhaustion and endpoints suddenly using unexpected DNS/gateway settings.

**Response considerations:** verify scope and impact, compare with known network changes, involve the network owner, preserve logs and use approved switch/DHCP controls. Do not disrupt a production VLAN based on a single noisy metric.

## 6. SOC triage checklist for a port/protocol alert

1. Bound the event by timestamp + timezone.
2. Capture src/dst IP and port, protocol, direction, action, bytes, duration and sensor.
3. Identify asset role, owner and expected communication path.
4. Compare frequency/volume to a baseline.
5. Correlate firewall, DNS, endpoint, authentication and DHCP logs where relevant.
6. Check whether the traffic succeeded, failed, or was only partially observed.
7. Record alternative explanations and any telemetry gap before closing/escalating.

## Practice prompt / Khud try karo

A synthetic endpoint sends hundreds of DNS queries to many unique subdomains, but no endpoint-process telemetry is available. Write a hunt note that separates observed facts from the DNS-tunnelling hypothesis, lists benign alternatives and identifies the next data source you would request.
