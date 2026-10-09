# Wireshark: The Basics — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Network Traffic Analysis**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Display filters, packet details, conversations and stream context ko samajhna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- A flow is described by endpoints, ports, protocol, direction, duration and volume; a port number alone is not proof of application identity.
- Display filters affect what is shown; capture filters affect what is collected. They are not interchangeable.
- DNS, TLS metadata and HTTP fields expose different context; encryption and sensor placement limit payload visibility.
- Packet loss, retransmissions, asymmetric routing, clock skew and NAT can change the apparent timeline.

## 3. Room-specific analyst lens

**Focus:** Display filters, packet details, conversations and stream context ko samajhna.

**Technical angle:** Understand display vs capture filters and packet list vs details. Record exact filter and frame numbers; keep the unfiltered source capture intact.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Hash/preserve the PCAP and record collection time, sensor location, size and known packet loss.
2. Start with protocol hierarchy, conversations, endpoints, duration and byte volume.
3. Choose one hypothesis and filter incrementally; keep the full conversation context available.
4. Inspect request/response pairs and packet fields; reconstruct streams only with missing-packet limitations in mind.
5. Correlate with resolver, firewall/proxy, Zeek/IDS and endpoint telemetry if authorised.
6. Record exact filters, frame references, tool version and what the capture cannot establish.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Scan-like fan-out | One source reaches many hosts/ports quickly. | Compare known scanners, admin jobs, NAT and connection-response patterns. |
| Possible exfiltration | Egress volume, destination or timing differs from baseline. | Link to identity/process and data sensitivity; backups/cloud sync are alternatives. |
| Unusual DNS | High rate, unusual labels or rare destinations. | Check resolver context, endpoint process, application baseline and capture completeness. |

## 6. Tooling and analyst data

Wireshark display filters: `dns`, `http.request`, `tcp.flags.syn == 1 && tcp.flags.ack == 0`. TShark summary: `tshark -r <authorised-capture.pcap> -q -z conv,tcp`. Verify syntax/version and preserve original capture.

Wireshark display filters (on authorised captures):
```text
dns
http.request
tcp.flags.syn == 1 && tcp.flags.ack == 0
```
TShark conversation summary: `tshark -r <authorised-capture.pcap> -q -z conv,tcp`.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Calling a large transfer exfiltration or rare domain malware without corroboration.
- Assuming encrypted payload data is visible.
- Publishing packet payloads or executing recovered files without permission/provenance.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

On a PCAP you are authorised to analyse, inventory three conversations, filter DNS and TCP setup traffic, then explain exactly what the capture shows versus what remains unknown.

**Room-specific task:** Understand display vs capture filters and packet list vs details. Record exact filter and frame numbers; keep the unfiltered source capture intact.

Produce an artefact register, one evidence-backed finding and a note describing what could not be established. Use synthetic or authorised data; do not query external targets as part of this exercise.

## 9. Evidence worksheet

| Time (UTC) | Source / artefact | Direct observation | Interpretation / confidence | Next pivot / owner |
|---|---|---|---|---|
| _Your observation_ | _File/event/frame/query_ | _What the record literally shows_ | _Fact vs inference; why this confidence_ | _Testable next action_ |
| _Corroborating item_ | _Independent source_ | _What it adds or contradicts_ | _Alternative explanation_ | _Owner / due time_ |

## 10. Report format

**Finding:** one plain-language sentence.  
**Scope:** assets/users/records and bounded time range.  
**Evidence:** source + timestamp + event/frame/document ID + query/filter.  
**Assessment:** confirmed observation, interpretation, confidence and benign alternative.  
**Impact:** what is affected and what remains unknown.  
**Action:** action taken, approval boundary, next owner and success verification.  
**Limitations:** missing logs, sampling, uncertain joins, tool constraints and follow-up evidence.

**Illustrative phrasing:** “The available records show [observation] during [window]. This supports [hypothesis] with [confidence] because [corroboration]. [Alternative] remains plausible because [gap]. Next, validate [specific fact] using [source/owner] before [response decision].” Replace placeholders with your own evidence.

## 11. Further reading

- [Wireshark display filter syntax](https://www.wireshark.org/docs/man-pages/wireshark-filter.html)
- [Wireshark display filter reference](https://www.wireshark.org/docs/dfref/)
- [Zeek logs reference](https://docs.zeek.org/en/current/reference/logs/index.html)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
