# IP and Domain Threat Intel — SOC L1 Field Notes

> Independent study sheet for **Threat Analysis Tools**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Threat Analysis Tools · **Sheet:** 00

## Room focus

IP/domain enrichment ko DNS, hosting, certificate, time and local telemetry ke saath interpret karna.

## Concepts to carry into the room

- Shared hosting, CDNs, VPNs and dynamic DNS can create ambiguous reputation.
- Historical context and observation time matter for rapidly changing infrastructure.
- Passive enrichment should be tied to the incident's own telemetry.

## Analyst lens

- Domain/IP, DNS answers and TTL, registration/certificate context, source and lookup time.
- Local resolution/connection events.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Record indicator, type, source, query timestamp, first/last seen, confidence, context and limitations. Passive enrichment is a lead, not proof. Shared hosting, CDNs, VPNs and dynamic infrastructure can make IP/domain reputation ambiguous.

## Common traps

- Blocking a shared IP based only on one threat label.
- Treating WHOIS registrant data as definitive attribution.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Is the indicator infrastructure shared by benign tenants?
- Do local logs show actual communication?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
