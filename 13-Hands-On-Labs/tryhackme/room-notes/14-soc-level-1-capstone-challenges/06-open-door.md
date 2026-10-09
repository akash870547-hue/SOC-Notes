# Open Door — SOC L1 Field Notes

> Independent study sheet for **SOC Level 1 Capstone Challenges**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** SOC Level 1 Capstone Challenges · **Sheet:** 00

## Room focus

A case narrative ko authorised scope, clear evidence and defensible response recommendations ke saath structure karna.

## Concepts to carry into the room

- Keep claims tied to artefacts and keep access scope explicit.
- Report impact and uncertainty in operational language.
- Recommendations must be proportionate and assigned to a role/owner.

## Analyst lens

- Initial alert, scope, timeline, asset/identity context and response status.
- Remaining gaps and requested action/owner.

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

- Writing a dramatic story unsupported by evidence.
- Suggesting actions without ownership or verification.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Can a reader trace each conclusion to evidence?
- Who needs to do what next, and how is success verified?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
