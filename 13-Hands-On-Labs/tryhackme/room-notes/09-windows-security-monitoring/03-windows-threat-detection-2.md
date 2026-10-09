# Windows Threat Detection 2 — SOC L1 Field Notes

> Independent study sheet for **Windows Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Windows Security Monitoring · **Sheet:** 00

## Room focus

Related Windows events ko timeline, parent-child relationship and identity changes ke across correlate karna.

## Concepts to carry into the room

- Process ancestry and command line need parent, user and path context.
- Account events across hosts may reveal a wider scope.
- Correlate endpoint records with network and identity telemetry when available.

## Analyst lens

- Process GUID/PID, parent, user/session, host, timestamp, command line and network connection.
- Cross-host identity and event sequence.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Example read-only query against a host you administer:

```powershell
Get-WinEvent -FilterHashtable @{ LogName = 'Security'; StartTime = (Get-Date).AddHours(-24) } -MaxEvents 100
```

Capture the host, query window and relevant event fields. Event IDs and available fields depend on audit policy, OS version and telemetry configuration.

## Common traps

- Correlating PIDs across long time ranges without process start context.
- Concluding lateral movement from a single remote logon event.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which entities link the records reliably?
- What alternative admin workflow fits the sequence?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
