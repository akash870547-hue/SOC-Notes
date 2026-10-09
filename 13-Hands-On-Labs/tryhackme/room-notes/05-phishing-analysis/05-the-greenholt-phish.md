# The Greenholt Phish — SOC L1 Field Notes

> Independent study sheet for **Phishing Analysis**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Phishing Analysis · **Sheet:** 00

## Room focus

A case-style phishing assessment ko fact/hypothesis separation aur an evidence-led summary ke saath likhna.

## Concepts to carry into the room

- The case conclusion must follow the artefacts you independently observed.
- Use the same verification standards for benign and malicious hypotheses.
- Do not publish room-specific answers or screenshots while the room is active.

## Analyst lens

- Observed sender/header details, message context, URL/attachment attributes and supporting source.
- Benign alternative and confidence rationale.

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

- Remembering a community answer instead of inspecting evidence.
- Writing a verdict without citing the artefact.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which artefact changed your assessment the most?
- What would you need to confirm recipient impact?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
