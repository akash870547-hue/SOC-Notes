# Network Security Essentials — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Network Security Monitoring**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Network controls, segmentation, firewall telemetry and normal communication patterns ka relationship samajhna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Firewall, flow, IDS and NSM records answer different questions; a deny event does not prove a successful session.
- Sensor placement, routing asymmetry, packet drops, TLS encryption, proxying and NAT constrain visibility.
- Scanning, MITM and exfiltration are hypotheses requiring behaviour plus network and business context.
- Test detections with positive, benign and edge traffic to avoid alert storms and blind spots.

## 3. Room-specific analyst lens

**Focus:** Network controls, segmentation, firewall telemetry and normal communication patterns ka relationship samajhna.

**Technical angle:** Record policy/rule, network zone, direction and allow/deny result; validate the actual path and exceptions rather than trusting a diagram.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Record sensor/interface, rule ID/version, direction, source/destination, protocol, time and action (alert/pass/drop).
2. Validate address/port semantics and any NAT, proxy or load-balancer transformations.
3. Establish baseline: unique peers/ports, rate, byte counts, duration and expected service communications.
4. Pivot to resolver, proxy, Zeek, endpoint and identity sources for corroboration.
5. Document confidence, control effect, benign alternative, scope and authorised next action.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Discovery | High target/port diversity. | Compare authorised scanner schedule, asset inventory, source role and response pattern. |
| Possible interception | Unexpected resolution, certificate or gateway change. | Validate trusted network design, TLS inspection and DHCP/ARP context. |
| Unusual egress | Destination/volume/timing differs from normal operations. | Correlate identity, process, data class and approved backup/cloud services. |

## 6. Tooling and analyst data

For Suricata, correlate EVE JSON `alert`, `flow` and protocol metadata only if the relevant outputs are enabled; validate whether traffic was merely observed or actively dropped. Fields depend on rules/configuration.

For Suricata, correlate EVE JSON alert/flow/protocol metadata if configured; distinguish alerting from blocking and preserve rule ID/revision.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Treating an IDS alert, blocked packet or high byte count as proof of compromise.
- Ignoring approved scans, NAT/shared IPs, backups and scheduled jobs.
- Tuning thresholds with no regression fixtures or recorded residual risk.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

Build a synthetic test matrix with one discovery pattern, one approved scanner and one benign bulk transfer. Identify fields that distinguish them and fields that are insufficient.

**Room-specific task:** Record policy/rule, network zone, direction and allow/deny result; validate the actual path and exceptions rather than trusting a diagram.

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

- [Suricata EVE JSON output](https://docs.suricata.io/en/latest/output/eve/eve-json-output.html)
- [Suricata rule management](https://docs.suricata.io/en/latest/rule-management/adding-your-own-rules.html)
- [Zeek common logs](https://docs.zeek.org/en/current/reference/logs/index.html)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
