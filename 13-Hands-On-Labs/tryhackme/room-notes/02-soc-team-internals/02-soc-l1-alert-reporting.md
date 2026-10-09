# SOC L1 Alert Reporting — SOC L1 Field Notes

> Independent study sheet for **SOC Team Internals**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SOC Team Internals · **Sheet:** 00

## Room focus

Concise, reproducible case notes aur evidence-based handoffs likhna.

## Concepts to carry into the room

- Lead with a plain-language summary, then supporting evidence.
- Distinguish observed facts, analyst interpretation and unanswered questions.
- Use UTC or an explicitly stated timezone consistently.

## Analyst lens

- Case ID, scope, timeline, entities, data sources and actions already taken.
- Owner, status, impact estimate and requested next action.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

No tool output is a verdict by itself. Write the case ID, analyst, time window, source system, status, confidence, business impact and next owner. Keep facts, hypotheses and actions separate.

## Common traps

- Speculation written as fact.
- Screenshots or summaries without searchable raw-event references.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Can another analyst reproduce the conclusion?
- Is the requested action explicit and authorised?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
