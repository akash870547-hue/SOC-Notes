# Cyber Kill Chain — SOC L1 Field Notes

> Independent study sheet for **Cyber Defence Frameworks**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Cyber Defence Frameworks · **Sheet:** 00

## Room focus

Observed activity ko high-level attack lifecycle ke stages ke saath map karna.

## Concepts to carry into the room

- Stages support communication but real intrusions can skip or repeat them.
- A stage hypothesis needs a specific observable artifact.
- Defenders can disrupt activity at multiple points, not just the first alert.

## Analyst lens

- Events supporting a possible stage, event time, asset and alternative explanation.
- Detection/prevention opportunity and evidence gaps.

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

- Forcing every event into one neat linear sequence.
- Claiming a stage based solely on an alert label.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What observation justifies the stage mapping?
- Where could a defensive control interrupt the chain?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
