# Introduction to SOAR — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Core SOC Solutions**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Playbooks, connectors, enrichment and controlled response automation ko evaluate karna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- SIEM centralises searchable events; its usefulness depends on collection, parsing, normalisation, retention and time alignment.
- EDR adds endpoint-focused telemetry such as process ancestry, file activity and network connections; policy determines visibility.
- SOAR orchestrates integrations and actions. It improves consistency but can amplify a bad decision without guardrails.
- A tool alert is a lead. Validate its underlying record and corroborate with another relevant source.

## 3. Room-specific analyst lens

**Focus:** Playbooks, connectors, enrichment and controlled response automation ko evaluate karna.

**Technical angle:** Document trigger → enrichment → branch → action → verification → audit. Include least privilege, retry/idempotency, timeout and failure/rollback paths.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Confirm source coverage, index/data view, time picker, permissions and ingestion delay.
2. Inspect representative raw records; validate field names and types before aggregation.
3. Pivot around stable identifiers (host ID, process GUID, request/case ID); qualify shared IPs and reusable usernames.
4. Use the next tool for the next question: process lineage, identity context, network visibility or response execution.
5. Record query/version, action execution state, failures and human approval for high-impact responses.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Unusual process lineage | Common executable with unusual parent, user, path or sequence. | Inspect signature, full path, command line, parent, user, host role and follow-on activity. |
| Cross-tool disagreement | Different timestamp, entity or verdict between products. | Check timezone, event vs ingest time, field mapping and identity normalisation. |
| Playbook reports success | Automation says completed but real control state is uncertain. | Verify action logs and actual endpoint/identity state; inspect partial failures. |

## 6. Tooling and analyst data

Splunk starter: `index=<authorised_index> earliest=-24h | stats count by sourcetype | sort - count`. Validate index and field availability; inspect records before trusting the result. Elastic: record data view and whether the query is KQL or Query DSL.

Splunk starter:
```spl
index=<authorised_index> earliest=-24h
| stats count by sourcetype
| sort - count
```
Elastic example only if ECS fields are present:
```text
event.category: authentication and event.outcome: failure
```
Confirm field mappings, raw events and time range before interpreting output.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Assuming advertised features mean telemetry is enabled and retained.
- Trusting a dashboard without inspecting events.
- Automating destructive actions without least privilege, idempotency, timeouts, approval and rollback.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

With synthetic authentication and process events, decide which questions belong in SIEM, which require endpoint context, and which response actions require a human approval gate.

**Room-specific task:** Document trigger → enrichment → branch → action → verification → audit. Include least privilege, retry/idempotency, timeout and failure/rollback paths.

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

- [Splunk stats](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Stats)
- [Splunk timechart](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Timechart)
- [Elastic KQL](https://www.elastic.co/guide/en/kibana/current/kuery-query.html)
- [Microsoft Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
