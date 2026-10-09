# SOC Role in Blue Team — SOC L1 Field Notes

> Independent study sheet for **Blue Team Introduction**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Blue Team Introduction · **Sheet:** 00

## Room focus

SOC ko detection, investigation, response, intelligence feedback aur cross-team coordination ke beech position karo.

## Concepts to carry into the room

- L1 identifies and scopes; escalation depends on playbook and authority.
- Asset owners provide business context; incident responders coordinate containment.
- Feedback from false positives and missed detections improves controls.

## Analyst lens

- Detection source, affected business service, owner, assigned severity and handoff time.
- Documented reason for escalation or closure.

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

- Assuming an analyst can isolate any asset without approval.
- Confusing a framework role with a company's exact job title.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What belongs to L1 versus incident response?
- What information should accompany an escalation?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
