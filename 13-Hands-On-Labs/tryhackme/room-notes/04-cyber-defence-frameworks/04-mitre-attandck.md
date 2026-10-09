# MITRE ATT&CK — SOC L1 Field Notes

> Independent study sheet for **Cyber Defence Frameworks**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Cyber Defence Frameworks · **Sheet:** 00

## Room focus

Evidence ko ATT&CK tactics/techniques se map karna and defensive coverage gaps ko explain karna.

## Concepts to carry into the room

- Tactics describe goals; techniques describe ways to achieve them.
- Mapping requires observable behaviour and version-aware references.
- Detection mapping and prevention mapping are different tasks.

## Analyst lens

- Source event, behaviour, technique hypothesis, confidence and reference version.
- Telemetry required to detect the behaviour.

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

- Mapping by tool name alone.
- Treating a technique mapping as attribution to a specific threat actor.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which exact behaviour supports the mapping?
- How could the mapping be disproved?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
