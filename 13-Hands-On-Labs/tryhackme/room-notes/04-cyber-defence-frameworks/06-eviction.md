# Eviction — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Cyber Defence Frameworks**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Eradication/recovery decisions ko evidence, scope and validation se justify karna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Pyramid of Pain compares how costly indicator categories may be to change; context and defender visibility determine operational value.
- Kill-chain models organise possible phases, but real intrusions can skip, repeat or overlap stages.
- ATT&CK tactics describe goals and techniques describe behaviours; a mapping is a hypothesis, not attribution.
- Begin with observed behaviour. Framework terminology cannot prove an event or fill a telemetry gap.

## 3. Room-specific analyst lens

**Focus:** Eradication/recovery decisions ko evidence, scope and validation se justify karna.

**Technical angle:** Separate containment, eradication and recovery. Verify credentials, persistence hypotheses, sibling scope and post-recovery monitoring.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Describe the observed behaviour in plain language without framework vocabulary.
2. Attach the source artefact, timestamp, entities and relevant fields.
3. Choose the narrowest plausible phase/technique and record alternative mappings.
4. Check that the technique's described behaviour matches the evidence and note framework version.
5. Map a defensive opportunity and the visibility gap that stops stronger confirmation.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Indicator match | A hash/domain/IP can provide a useful pivot. | Check freshness, source quality, shared infrastructure and local contact. |
| Technique-like behaviour | A sequence resembles a known adversary behaviour. | Name the observable behaviour and required telemetry; do not map from tool name alone. |
| Coverage gap | No reliable data source detects a behaviour. | Record required telemetry, cost, privacy implications and validation plan. |

## 6. Tooling and analyst data

Mapping record: observed behaviour → artefact → candidate phase/technique → confidence → benign alternative → telemetry gap → defensive opportunity.

Framework evidence record: observed behaviour → source artefact → candidate technique/phase → confidence → benign alternative → visibility gap → defensive opportunity.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Mapping many techniques to inflate perceived sophistication.
- Using a framework label as proof or actor attribution.
- Ranking indicators mechanically without local false-positive and visibility context.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

Map a synthetic admin sequence only as far as the evidence supports. Mark an expected but unobserved phase as unknown and name a data source that could test it.

**Room-specific task:** Separate containment, eradication and recovery. Verify credentials, persistence hypotheses, sibling scope and post-recovery monitoring.

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

- [MITRE ATT&CK techniques](https://attack.mitre.org/techniques/)
- [Sigma rules documentation](https://sigmahq.io/docs/basics/rules.html)
- [Cyber Kill Chain overview](https://www.lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/Cyber-Kill-Chain.pdf)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
