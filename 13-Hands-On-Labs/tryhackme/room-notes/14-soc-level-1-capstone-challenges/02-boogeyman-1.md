# Boogeyman 1 — SOC L1 Field Notes

> Independent study sheet for **SOC Level 1 Capstone Challenges**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SOC Level 1 Capstone Challenges · **Sheet:** 00

## Room focus

Scenario artefacts ko isolated clues ke bajay linked evidence set ke roop mein dekhna.

## Concepts to carry into the room

- Start from the artefact metadata and define the investigation window.
- Build links only where identifiers, timing or context support them.
- Preserve uncertainty around missing stages.

## Analyst lens

- Artifact source/hash, timestamps, account/host identifiers and cross-source links.
- Alternative explanation and unanswered question.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Use one timeline row per material observation: UTC time | artefact/source | observed fact | interpretation | confidence | next action. Close with scope, impact, containment/recovery status, evidence gaps, detection opportunities and owner for each follow-up.

## Common traps

- Assuming one artefact explains the entire incident.
- Copying solution narratives instead of validating the evidence.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which two artefacts are actually connected by evidence?
- What remains an inference?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
