# SOC Workbooks and Lookups — SOC L1 Field Notes

> Independent study sheet for **SOC Team Internals**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SOC Team Internals · **Sheet:** 00

## Room focus

Repeatable enrichment and investigation references ko transparent, maintained aur scoped rakhna.

## Concepts to carry into the room

- A workbook/playbook guides decisions but does not replace judgement.
- Lookup data needs provenance, refresh time and clear join keys.
- Record defaults and failure cases when an enrichment source is unavailable.

## Analyst lens

- Input fields, matching key, lookup source, update time and returned context.
- Example where the lookup is empty, stale or ambiguous.

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

- Treating a missing lookup result as proof of safety.
- Joining records on a non-unique field without validation.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What happens when an enrichment fails?
- How will another analyst know where the data came from?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
