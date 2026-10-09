# Benign — SOC L1 Field Notes

> Independent study sheet for **SIEM Triage for SOC**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SIEM Triage for SOC · **Sheet:** 00

## Room focus

Benign-looking or false-positive candidates ko evidence aur baseline se validate karna.

## Concepts to carry into the room

- Benign is an evidence-based verdict, not an absence of suspicion.
- Document why activity is expected and what residual risk remains.
- False-positive feedback should improve rules without creating blind spots.

## Analyst lens

- Expected process/user/asset role, approved change context and matching telemetry.
- Rule condition and reason it matched.

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

- Closing only because an activity looks familiar.
- Over-tuning a rule to a single benign case.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What evidence positively supports a benign verdict?
- What malicious variant could the proposed tuning miss?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
