# Windows Logging for SOC — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Windows Security Monitoring**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Windows event channels, providers, logon/process context and event-time semantics ka foundation banana.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Event 4624 records successful logon and 4625 failed logon; account, source, logon type and host context matter.
- Event 4688 can record process creation when configured; command-line visibility depends on policy and collection.
- Sysmon provides configurable telemetry, so absence of an event may be a collection/configuration issue.
- Separate event time from ingestion time; check OS version, time zone, retention and sensor health.

## 3. Room-specific analyst lens

**Focus:** Windows event channels, providers, logon/process context and event-time semantics ka foundation banana.

**Technical angle:** Capture channel/provider, full event fields, audit policy and ingest time. Event documentation provides meaning beyond the numeric ID.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Confirm host, channel/provider, event/ingestion time, audit policy and expected source coverage.
2. Inspect full event fields/XML: SID/name, logon type, source, process/parent identifiers and available command line.
3. Correlate authentication, process starts, task/service changes, DNS/network and privilege events.
4. Build a short UTC timeline; compare with RMM, service accounts, scheduled tasks and maintenance.
5. Report the strongest direct observation, missing telemetry and approved escalation/containment path.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Failure burst then success | Password guessing, stale credentials or ordinary mistyping may fit. | Check source, account diversity, MFA, logon type, lockout and nearby host/identity events. |
| Unusual process tree | Known executable appears with unusual parent, user, path or follow-on network. | Validate full path/signature, command line, parent, prevalence and business workflow. |
| Missing events | Expected event type is absent or delayed. | Inspect audit policy, Sysmon config, agent health, forwarding and clock drift. |

## 6. Tooling and analyst data

Read-only PowerShell starter on a host you administer: `Get-WinEvent -FilterHashtable @{ LogName='Security'; StartTime=(Get-Date).AddHours(-24) } -MaxEvents 100`. Scope the window and inspect raw fields before drawing conclusions.

Read-only PowerShell starter for a host you administer:
```powershell
Get-WinEvent -FilterHashtable @{ LogName='Security'; StartTime=(Get-Date).AddHours(-24) } -MaxEvents 100
```

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Memorising event IDs without field/context interpretation.
- Treating missing logs as proof no event occurred.
- Joining only on PID over long intervals without host/start context or process GUID.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

On an authorised Windows VM, review a bounded event window, preserve two complete event references and compare normal vs suspicious hypotheses. Do not alter audit settings or enact response without approval.

**Room-specific task:** Capture channel/provider, full event fields, audit policy and ingest time. Event documentation provides meaning beyond the numeric ID.

Produce an artefact register, one evidence-backed finding and a note describing what could not be established. Use synthetic or authorised data; do not query external targets as part of this exercise.

## 9. Evidence worksheet

| Time (UTC) | Source / artefact | Direct observation | Interpretation / confidence | Next pivot / owner |
|---|---|---|---|---|
| _Your observation_ | _File/event/frame/query_ | _What the record literally shows_ | _Fact vs inference; why this confidence_ | _Testable next action_ |
| _Corroborating item_ | _Independent source_ | _What it adds or contradicts_ | _Alternative explanation_ | _Owner / due time_ |

## 10. Report format

**Finding:** one plain-language sentence.  
**Scope:** assets/users/records and bounded time range.  
**Evidence:** source + timestamp + event/frame/document ID + query/filter.  
**Assessment:** confirmed observation, interpretation, confidence and benign alternative.  
**Impact:** what is affected and what remains unknown.  
**Action:** action taken, approval boundary, next owner and success verification.  
**Limitations:** missing logs, sampling, uncertain joins, tool constraints and follow-up evidence.

**Illustrative phrasing:** “The available records show [observation] during [window]. This supports [hypothesis] with [confidence] because [corroboration]. [Alternative] remains plausible because [gap]. Next, validate [specific fact] using [source/owner] before [response decision].” Replace placeholders with your own evidence.

## 11. Further reading

- [Microsoft Event 4624](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4624)
- [Microsoft Event 4625](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4625)
- [Microsoft Event 4688](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4688)
- [Microsoft Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
