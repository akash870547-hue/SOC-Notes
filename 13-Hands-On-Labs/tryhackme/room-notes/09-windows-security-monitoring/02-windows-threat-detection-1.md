# Windows Threat Detection 1 — SOC L1 Field Notes

> Independent study sheet for **Windows Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Windows Security Monitoring · **Sheet:** 00

## Room focus

Windows telemetry se clear hypothesis banane aur process/account context check karne ki practice.

## Concepts to carry into the room

- One event rarely explains the full sequence.
- Identity, logon type, host role and nearby process/network activity change interpretation.
- Use a bounded window and retain raw event references.

## Analyst lens

- Account, host, event time, logon/process context and adjacent records.
- Expected administrative or service activity.

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

- Overfitting to a familiar event ID.
- Ignoring service accounts and scheduled automation.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What baseline explains this event?
- What related event could confirm the hypothesis?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
