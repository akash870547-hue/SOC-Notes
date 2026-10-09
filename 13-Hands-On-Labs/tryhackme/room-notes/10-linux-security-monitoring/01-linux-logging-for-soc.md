# Linux Logging for SOC — SOC L1 Field Notes

> Independent study sheet for **Linux Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Linux Security Monitoring · **Sheet:** 00

## Room focus

Authentication, systemd journal, sudo and service logs ki source and reliability samajhna.

## Concepts to carry into the room

- Different distributions store logs in different locations and formats.
- Authentication messages need user, source, service and time context.
- journald retention and forwarding configuration affect visibility.

## Analyst lens

- Auth events, sudo records, journal unit, hostname, UID and timestamp.
- Collection configuration and time sync.

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

- Assuming every distro uses identical log paths.
- Treating missing auth logs as no login activity.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which source records the event on this host?
- Could log rotation or forwarding explain missing data?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
