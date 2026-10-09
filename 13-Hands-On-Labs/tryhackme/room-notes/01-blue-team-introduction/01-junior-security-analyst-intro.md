# Junior Security Analyst Intro — SOC L1 Field Notes

> Independent study sheet for **Blue Team Introduction**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Blue Team Introduction · **Sheet:** 00

## Room focus

L1 analyst ke daily work, queue ownership, escalation aur clear communication ko map karo.

## Concepts to carry into the room

- Triage means quick scope-and-prioritise, not instant certainty.
- Separate alert severity from confidence in the verdict.
- A handoff must let the next analyst continue without redoing the whole case.

## Analyst lens

- Alert source, rule, severity, timestamp and asset/user context.
- What was checked, what remains unknown, and why the case moved queues.

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

- Treating every alert as an incident.
- Closing without a reason or next action.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What information must a ticket contain before escalation?
- Which missing fact would most change the next action?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
