# 04 · Network Traffic Analysis — Field Notes

**Best paired with:** [Network Traffic Basics](https://tryhackme.com/room/networktrafficbasics), [Wireshark: The Basics](https://tryhackme.com/room/wiresharkthebasics), [Wireshark: Packet Operations](https://tryhackme.com/room/wiresharkpacketoperations), [Wireshark: Traffic Analysis](https://tryhackme.com/room/wiresharktrafficanalysis), [Network Security Essentials](https://tryhackme.com/room/networksecurityessentials), and [Data Exfiltration Detection](https://tryhackme.com/room/dataexfildetection).

These are general blue-team notes for authorised lab captures and synthetic traffic. They do not reconstruct any room's answers or challenge path.

## Start with a hypothesis

Examples of defensible questions:
- Is one host making repeated connections to an unusual destination?
- Are DNS queries inconsistent with the host's baseline?
- Does a connection show a successful exchange or only attempted traffic?
- Could the observed traffic be explained by an approved agent, update service, CDN or administrative workflow?

## A small Wireshark filter shelf

~~~text
ip.addr == 10.10.10.20
dns
dns.flags.response == 0
tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.stream eq 0
http.request
tcp.port == 443
~~~

These filters isolate traffic for review; they do not diagnose maliciousness by themselves. Change IPs and stream numbers to values from your own lab capture. Application visibility depends on protocol, encryption and capture location.

## Analysis sequence

1. **Capture context:** source, collection point, time range, timezone and whether packet loss is known.
2. **Protocol hierarchy:** identify which protocols and conversations dominate the capture.
3. **Conversations / endpoints:** compare top talkers, direction, packet counts, bytes and duration.
4. **DNS:** review queried names, record types, response patterns and which client initiated them.
5. **TCP/session behaviour:** inspect handshakes, resets, retransmissions, unusual connection cadence and successful exchanges.
6. **Application context:** inspect HTTP metadata where available; encrypted HTTPS generally limits payload visibility unless approved decryption or endpoint data exists.
7. **Correlate:** connect network observations to process, identity, proxy and firewall logs.
8. **Conclude carefully:** report observation, hypothesis, confidence, gaps and next pivot.

## Common indicators to validate, not blindly label

| Observation | Alternative explanation to check |
|---|---|
| Many SYNs without completed sessions | Scanning, packet loss, health checks, load balancers |
| High DNS query volume | Agent telemetry, CDN, browser/app retries, software updates |
| Long/high-entropy DNS labels | Encoded-data hypothesis, but also legitimate service labels or telemetry |
| Large outbound transfer | Approved backup, sync, cloud upload or business data processing |
| Repeated connection intervals | Polling/monitoring agents or normal application heartbeat |
| TLS certificate / SNI anomaly | Misconfiguration, interception, new service or incomplete sensor visibility |

## Analyst capture note

~~~text
Capture / evidence ID:
Sensor and collection point:
Time range + timezone:
Hosts and IPs in scope:
Top protocols / conversations:
DNS observations:
TCP/session observations:
Application visibility limits:
Related endpoint / identity / firewall events:
Hypothesis and benign alternatives:
Assessment + confidence:
Next step:
~~~

## Hinglish takeaway

Packet mein jo dikh raha hai woh record karo; jo hidden hai usko imagine mat karo. Encrypted traffic ke case mein metadata aur endpoint context ka correlation particularly important hota hai.
