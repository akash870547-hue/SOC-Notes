# ItsyBitsy — SOC L1 Field Notes

> Independent study sheet for **SIEM Triage for SOC**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SIEM Triage for SOC · **Sheet:** 00

## Room focus

Small-scope scenario triage ko structured evidence, timeline and proportionate confidence ke saath practise karna.

## Concepts to carry into the room

- A short case still benefits from scope, timeline and evidence citations.
- Small details can be misleading without baseline context.
- State uncertainty rather than adding unsupported narrative.

## Analyst lens

- Event chain, involved entities, raw log references and initial alert context.
- Supporting and contradicting evidence.

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

- Rushing because the scenario appears small.
- Mistaking sequence for causation.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What is the minimum defensible conclusion?
- Which missing log would most reduce uncertainty?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
