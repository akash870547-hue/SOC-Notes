# Phishing Unfolding — SOC L1 Field Notes

> Independent study sheet for **Phishing Analysis**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Phishing Analysis · **Sheet:** 00

## Room focus

A developing phishing scenario ko evolving evidence, scope changes and status updates ke through analyse karna.

## Concepts to carry into the room

- Update the hypothesis as new evidence arrives; preserve earlier rationale.
- Correlate mail telemetry with identity and endpoint signals when available.
- Communicate scope changes and decision points.

## Analyst lens

- Initial report, new indicators, affected recipients/accounts and a UTC timeline.
- Evidence that changed confidence or response priority.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Safe first-pass checks: preserve the original message and headers where permitted; inspect sender/Reply-To/Return-Path and authentication results; expand URL text without visiting the destination; record attachment filename/type/hash; correlate with user report and mail telemetry. Do not upload confidential artefacts to unapproved public services.

## Common traps

- Forcing all new observations to fit the first hypothesis.
- Losing the difference between initial and updated findings.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What new evidence changed the scope?
- Which actions require immediate escalation?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
