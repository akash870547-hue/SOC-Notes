# Pyramid of Pain — SOC L1 Field Notes

> Independent study sheet for **Cyber Defence Frameworks**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Cyber Defence Frameworks · **Sheet:** 00

## Room focus

Indicators ko attacker ke liye change karne ki cost aur defender ke detection value se compare karna.

## Concepts to carry into the room

- Hashes and isolated indicators can be easy to change; behaviour and TTPs are generally more durable.
- Context determines usefulness: no indicator is universally conclusive.
- Higher-level detections still need precise, testable logic.

## Analyst lens

- Indicator type, source/reliability, observation window and observed behaviour.
- What would make the indicator stale or easy to evade?

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Use a compact mapping table: observed behaviour → supporting artefact → framework phase/technique hypothesis → confidence → detection/control opportunity. Link the exact behaviour, not just the tool or alert label.

## Common traps

- Treating the pyramid as a strict ranking for every case.
- Blocking a shared or benign indicator without context.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which observation is most durable here, and why?
- How could the detection be validated safely?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
