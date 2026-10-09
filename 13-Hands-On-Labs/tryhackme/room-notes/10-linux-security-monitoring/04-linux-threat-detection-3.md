# Linux Threat Detection 3 — SOC L1 Field Notes

> Independent study sheet for **Linux Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Linux Security Monitoring · **Sheet:** 00

## Room focus

Host investigation findings ko timeline, impact and remaining telemetry gaps ke saath report karna.

## Concepts to carry into the room

- Combine relevant auth, process, service, filesystem and network records.
- Document command results as observations, including the host and time queried.
- Recommend reversible and authorised actions where possible.

## Analyst lens

- Timeline, host/user scope, log source, process/file metadata and network activity.
- Integrity/provenance notes and response status.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Read-only local triage examples:

```bash
journalctl --since "24 hours ago"
grep -E 'Failed password|Accepted password' /var/log/auth.log
```

Log paths differ across distributions and may require privileges; preserve timestamps and note rotation/forwarding gaps.

## Common traps

- Copying outputs with no command/time context.
- Overlooking container or cloud-instance identity.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Could another analyst reproduce the observation?
- What evidence is still needed before closure?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
