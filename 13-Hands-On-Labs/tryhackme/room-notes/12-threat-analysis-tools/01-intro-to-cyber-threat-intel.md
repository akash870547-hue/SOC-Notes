# Intro to Cyber Threat Intel — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Threat Analysis Tools**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Intelligence lifecycle, collection context, source reliability and confidence ka practical use.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Reputation/enrichment adds context; it does not automatically confirm compromise.
- IP/domain information changes over time; hash reputation is specific to exact bytes and source freshness.
- Shared hosting, CDNs, VPNs, NAT and dynamic DNS can make infrastructure reputation ambiguous.
- Threat intelligence guides a testable local hypothesis; preserve source, time, confidence and limitations.

## 3. Room-specific analyst lens

**Focus:** Intelligence lifecycle, collection context, source reliability and confidence ka practical use.

**Technical angle:** Record source, collection method, first/last seen, confidence and sharing limits; convert external intel into local DNS/proxy/EDR/mail tests.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Normalise the indicator but preserve original input and indicator type.
2. Query only authorised services; log source, time and the exact input.
3. Compare first/last seen, confidence, source methodology, context and independent agreement.
4. Pivot into local DNS, proxy, EDR, mail or identity logs to confirm relevance to this environment.
5. Write what enrichment establishes, what it cannot establish and the next local evidence required.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Reputation match | Source associates indicator with previous activity. | Check freshness, source reliability, shared infrastructure and actual local contact. |
| Rare/new domain | Could reflect new infrastructure or legitimate newly deployed software. | Compare enterprise baseline, approved services, CDN/software updater and DNS telemetry. |
| Conflicting sources | Different vendors label the same indicator differently. | Preserve disagreement; seek direct local evidence instead of averaging scores. |

## 6. Tooling and analyst data

Enrichment record: indicator, type, source, queried UTC, first/last seen, confidence, context, limitations and local hits. Do not upload secrets, customer artifacts or personal data to public tools.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Treating registration data as attribution.
- Using an old report as proof of current maliciousness.
- Blocking shared infrastructure because of one score without risk/impact review.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

Compare three synthetic indicators: stale malicious label, shared-cloud IP and newly observed domain. Separate external intelligence from local telemetry and justify confidence.

**Room-specific task:** Record source, collection method, first/last seen, confidence and sharing limits; convert external intel into local DNS/proxy/EDR/mail tests.

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

- [NIST SP 800-150](https://csrc.nist.gov/pubs/sp/800/150/final)
- [MITRE ATT&CK techniques](https://attack.mitre.org/techniques/)
- [Sigma rules](https://sigmahq.io/docs/basics/rules.html)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
