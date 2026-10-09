# Alert Triage With Splunk — SOC L1 Field Notes

> Independent study sheet for **SIEM Triage for SOC**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SIEM Triage for SOC · **Sheet:** 00

## Room focus

Splunk observations ko triage verdict, supporting evidence and next action mein translate karna.

## Concepts to carry into the room

- Search should be time-bounded and driven by a clear hypothesis.
- Look at raw events around the alert; use stats as a navigation aid.
- Record query text and assumptions for reproducibility.

## Analyst lens

- Alert payload, SPL, matching raw events, entity pivots and time window.
- Benign counterpart and remaining uncertainty.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Example SPL (replace the placeholder with a dataset you are authorised to query):

```spl
index=<authorized_index> earliest=-24h
| stats count by sourcetype
| sort - count
```

Validate the index, time bounds and fields first; inspect representative raw events before relying on aggregations. For Elastic, record the data view and whether the query is KQL or DSL.

## Common traps

- Accepting alert title as the verdict.
- Expanding scope without documenting why.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which record is the strongest corroboration?
- What would change the triage status?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
