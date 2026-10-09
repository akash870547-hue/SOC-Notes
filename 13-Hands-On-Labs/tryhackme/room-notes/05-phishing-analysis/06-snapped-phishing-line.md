# Snapped Phishing Line — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Phishing Analysis**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

User-reported suspicious communication ko triage, scope and hand off karna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Display-name spoofing, lookalike domains and compromise of a legitimate sender account are different cases.
- SPF, DKIM and DMARC answer related but distinct questions; interpret them with alignment, forwarding and mail-gateway context.
- A visible URL may differ from its destination, redirect or be session-dependent; do not visit a suspect link to inspect it.
- Preserve original message/headers where permitted; never upload sensitive artefacts to an unapproved public service.

## 3. Room-specific analyst lens

**Focus:** User-reported suspicious communication ko triage, scope and hand off karna.

**Technical angle:** Capture original message, report time, whether it was clicked/replied to and whether credentials or files were submitted. Distinguish classification from exposure scope.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Preserve the original message and note report channel, recipient and report time.
2. Inspect From, Reply-To, Return-Path and Received chain; identify which mail system added each header.
3. Interpret authentication results with domain alignment and known forwarding/provider setup.
4. Extract URL and attachment metadata safely; compare requested action with normal business process.
5. Scope similar messages, recipients, clicks, submitted credentials and related identity/endpoint evidence.
6. Write verdict/confidence, affected users, containment options and unknowns.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Identity mismatch | Sender/reply/signing domains do not align as expected. | Check approved mailing services, forwarding, delegated domains and third-party platforms. |
| High-risk request | Urgency or unusual payment/credential action diverges from established process. | Verify via a trusted independent channel and preserve business context. |
| URL/attachment clue | Lookalike host, unusual archive/script or unexpected metadata. | Use safe passive analysis and corroborating gateway/endpoint evidence. |

## 6. Tooling and analyst data

Preserve sender fields, received chain, authentication results, URL/attachment metadata, mail delivery context, recipient action and related mail/identity/endpoint events. A reputation score is one clue, not a verdict.

Check From, Reply-To, Return-Path, Received chain, authentication alignment, URL/attachment metadata, reporter context and related mail/identity/endpoint telemetry. Authentication failures need context; preserve the original message.

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Declaring phishing from grammar, display-name or one SPF result alone.
- Opening suspect attachments or testing links from an analyst workstation.
- Forwarding a message in a way that discards headers or uploading sensitive samples without permission.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

Use a synthetic email header and mock user report. Mark facts vs interpretations; list the independent evidence that would change your confidence. Don't use a real mailbox or unapproved live target.

**Room-specific task:** Capture original message, report time, whether it was clicked/replied to and whether credentials or files were submitted. Distinguish classification from exposure scope.

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

- [CISA — Recognize and Report Phishing](https://www.cisa.gov/secure-our-world/recognize-and-report-phishing)
- [RFC 5322 Internet Message Format](https://www.rfc-editor.org/rfc/rfc5322)
- [RFC 7489 DMARC](https://www.rfc-editor.org/rfc/rfc7489)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
