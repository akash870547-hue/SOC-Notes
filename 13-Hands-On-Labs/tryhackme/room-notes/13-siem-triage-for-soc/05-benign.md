# Benign — SOC L1 Deep-Dive Notes

> **Independent analyst companion · SIEM Triage for SOC**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Benign-looking or false-positive candidates ko evidence aur baseline se validate karna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Data model matters: index/data view, source type, parsing, normalisation, retention and ingestion latency can change results.
- Use a narrow time window and inspect raw records before aggregation.
- Aggregations help prioritise but can hide the event that explains a case.
- Joins need stable keys plus time context; shared IPs and usernames can be ambiguous.

## 3. Room-specific analyst lens

**Focus:** Benign-looking or false-positive candidates ko evidence aur baseline se validate karna.

**Technical angle:** A benign verdict requires positive evidence of expected ownership/workflow; record the rule condition and regression test before tuning.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Confirm the correct index/data view and demonstrate that expected data exists in the requested window.
2. Write the hypothesis and query, then inspect sample raw events before aggregation.
3. Establish baseline counts by host/user/source/status/time bucket.
4. Pivot with stable identifiers and validate timezone, nulls, data types and duplicate records.
5. Save query, parameters, event references and limitations in the case for reproducibility.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Burst/sequence | Counts cluster in time or differ sharply for one entity. | Compare raw records, time buckets, baseline and detector intent. |
| Cross-source link | Identity, endpoint and network events seem connected. | Validate identifiers, time alignment, ingest delay, NAT and duplicated events. |
| Zero results | Search returned no matching rows. | Check spelling, mappings, time window, data view, permissions, retention and source health. |

## 6. Tooling and analyst data

Splunk starter: `index=<authorised_index> earliest=-24h | stats count by sourcetype | sort - count`. Elastic example only when ECS fields exist: `event.category: authentication and event.outcome: failure`. Inspect documents and field types first.

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

- Starting with visualisations before validating source records.
- Copying other environment's field/index names unchanged.
- Expanding searches without recording why or exposing unnecessary data.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

With a synthetic CSV/lab dataset, record the base query, three raw event references, one aggregate and one limitation. Change a single filter and explain how the cohort changes.

**Room-specific task:** A benign verdict requires positive evidence of expected ownership/workflow; record the rule condition and regression test before tuning.

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
- [Sigma rules](https://sigmahq.io/docs/basics/rules.html)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
