# Junior Security Analyst Intro — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Blue Team Introduction**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

L1 analyst ke daily work, queue ownership, escalation aur clear communication ko map karo.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- People, process and technology must work together; telemetry without an escalation path is not an operational capability.
- Detection, triage, containment, eradication and recovery are different activities with different authorities.
- Priority should combine confidence, impact, exposure and urgency; a severity label is not a case conclusion.
- The analyst's job is to reduce uncertainty and preserve a defensible record, not to invent a complete story.

## 3. Room-specific analyst lens

**Focus:** L1 analyst ke daily work, queue ownership, escalation aur clear communication ko map karo.

**Technical angle:** Queue hygiene: accepted → investigating → waiting on context → escalated/closed. Each state change needs an owner, timestamp and reason.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Identify the alert owner, source, time zone, affected identity/asset and business service at risk.
2. Write the question being answered in one sentence; state the suspected behaviour, not a verdict.
3. Check source health, timestamp alignment and whether the relevant records were collected.
4. Compare activity with account/asset role, expected workflow, maintenance window and known administration paths.
5. Choose a playbook-approved next action; state the owner, reason and verification condition.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Role or asset mismatch | Could indicate unexpected access or merely a special business workflow. | Confirm account privilege, asset purpose, approved admin path and change records. |
| Unexpected human request | Urgency or novelty becomes important when identity or requested action is unusual. | Verify independently through a trusted channel; do not rely on display names. |
| Exposure/configuration gap | Raises risk but does not establish exploitation. | Separate exposure, attempted use, successful execution and impact. |

## 6. Tooling and analyst data

No universal query replaces process. Record case ID, source, first/last seen, entities, severity, confidence, status, evidence references, action, owner and closure reason.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Treating the SOC as a single person who owns every decision.
- Calling an event malicious because it is unfamiliar, or benign because it looks familiar.
- Escalating with no question, evidence, impact estimate or requested action.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

Build a synthetic queue with two expected admin events, one noisy rule, one high-impact/low-confidence case and one corroborated suspicious sequence. Rank them, justify uncertainty and assign the next owner without relying on room answers.

**Room-specific task:** Queue hygiene: accepted → investigating → waiting on context → escalated/closed. Each state change needs an owner, timestamp and reason.

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

- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
- [NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)
- [CISA — Recognize and Report Phishing](https://www.cisa.gov/secure-our-world/recognize-and-report-phishing)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
