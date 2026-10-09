# Detecting Web Shells — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Web Security Monitoring**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Unexpected server-side files, process behaviour and web-request patterns ko correlate karna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- A status code alone does not prove success: 200 can be an error page, 404 can be normal crawling and 500 can be an application defect.
- Source IP may be a proxy/CDN/load balancer; forwarded-client fields are trustworthy only behind a known proxy chain.
- Web server, WAF, application, identity, process and filesystem logs answer complementary questions.
- High confidence needs a sequence, session/account, outcome, downstream side effects and deployment context.

## 3. Room-specific analyst lens

**Focus:** Unexpected server-side files, process behaviour and web-request patterns ko correlate karna.

**Technical angle:** Correlate file metadata/hash/owner with web requests, process ancestry and outbound connections. Preserve evidence before authorised containment.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Validate format, timezone, request ID, proxy topology, retention and log sampling/drop behaviour.
2. Baseline method, route, status, response size, user/session and rate by small intervals.
3. Investigate sequences and correlated errors, not just one suspicious string.
4. Join to WAF/application/host events using request IDs or reliable identities and bounded time windows.
5. Preserve artefact metadata and escalate based on evidence of impact, persistence, exposure or service degradation.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Route/parameter probing | Changing paths, repeated errors or unusual methods. | Compare scanners, health checks, tests and server-side result. |
| Unexpected child process | Web worker launches tools outside its expected role. | Inspect ancestry, account, command line, deployment and filesystem changes. |
| Traffic flood | Request rate coincides with latency/error growth. | Check edge and upstream health; weigh legitimate spikes and mitigation collateral impact. |

## 6. Tooling and analyst data

Group authorised access logs by route/status and short time bucket; pivot by request ID, session and app logs. Keep the original raw log and identify which layer created each field.

For web logs, group by route, method, status, size, user/session and short time bucket. Correlate request ID to WAF/application/host records. A response code alone does not prove impact.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Declaring exploit success from a payload-looking request only.
- Trusting arbitrary forwarded IP headers.
- Deleting suspicious files or rebooting before evidence preservation under the incident plan.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

Use synthetic access logs with normal traffic, a benign scanner, route probing and an app error. State which additional logs would be required to confirm a successful exploit.

**Room-specific task:** Correlate file metadata/hash/owner with web requests, process ancestry and outbound connections. Preserve evidence before authorised containment.

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

- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
