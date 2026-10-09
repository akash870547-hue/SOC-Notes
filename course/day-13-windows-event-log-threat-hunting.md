# Day 13 — Windows Event Log Threat Hunting

> **Source status:** The supplied text includes Day 13 in the curriculum summary but does not include a detailed Day 13 chapter. This chapter expands the listed Windows Event IDs and account-compromise timeline objective.

> **What you'll learn / Is chapter mein:** Correlate Windows authentication and process events to investigate suspicious logons, privilege use and account-compromise hypotheses.
>
> **Seedhi baat:** Ek Event ID apne aap incident ka certificate nahi hota. Account, logon type, host, source IP, process lineage aur time sequence ko saath dekhna padta hai.

## 1. Event ID quick reference

| Event ID | Common meaning | Investigation pivots |
|---|---|---|
| 4624 | Successful logon | Target account, logon type, source/network fields, host, time |
| 4625 | Failed logon | Account, source, failure status/substatus, repetition pattern |
| 4672 | Special privileges assigned to a new logon | Account, logon session, expected admin role, event sequence |
| 4688 | Process creation | Process/creator, command line if configured, parent process if available |
| 1102 | Security audit log cleared | Subject, host, time, nearby admin/change events and log forwarding gaps |

Field names and availability depend on OS version, audit policy and log parser. Inspect the raw XML/event details and verify event collection before querying.

## 2. Build a timeline before drawing conclusions

A useful sequence could look like this:

```text
Failed logons (4625)
       ↓
Successful logon (4624)
       ↓
Privileged logon context (4672, if generated)
       ↓
Unexpected process execution (4688, if collected)
       ↓
Possible log clearing (1102) or other correlated activity
```

This is an investigative pattern, not a required sequence. These events may be missing, generated on different systems, delayed in ingestion, or unrelated to one another.

## 3. Logon types — important context

Common Windows logon types include:
- **2 — Interactive:** usually a local console logon.
- **3 — Network:** access over the network; meaning depends on the service/protocol.
- **5 — Service:** service-control manager context.
- **7 — Unlock:** workstation unlock.
- **10 — RemoteInteractive:** commonly Remote Desktop.
- **11 — CachedInteractive:** cached domain credentials used for interactive sign-in.

Treat this as a quick reference and validate against the event details / official Windows documentation for the system you investigate. A type alone does not determine whether an activity is malicious.

## 4. Example Splunk searches (schema-dependent)

### Failed logons grouped by account and source

~~~spl
index=lab earliest=-24h EventCode=4625
| stats count as failures min(_time) as first_seen max(_time) as last_seen values(Computer) as hosts by TargetUserName, IpAddress
| sort - failures
~~~

### Successful logons and privileged logon context

~~~spl
index=lab earliest=-24h EventCode IN (4624,4672)
| table _time Computer SubjectUserName TargetUserName EventCode LogonType IpAddress LogonId
| sort 0 _time
~~~

### Process-creation context

~~~spl
index=lab earliest=-24h EventCode=4688
| table _time Computer SubjectUserName NewProcessName ProcessCommandLine ParentProcessName
| sort 0 _time
~~~

These queries are templates, not drop-in production rules. Confirm field mapping, index, source and local Splunk syntax before relying on results.

## 5. Investigation workflow

1. **Validate the alert:** Identify the event source, hostname, account and precise time window.
2. **Check the account:** Human/service identity, expected location, privilege level, normal hours and recent changes.
3. **Compare failures and successes:** Are the same accounts/source/host involved? Could VPN, NAT or a legitimate password issue explain the pattern?
4. **Review privilege evidence:** Determine whether elevated privileges were expected for this identity and session.
5. **Pivot to processes:** Look at 4688/Sysmon/EDR for unexpected shells, scripting, tools or parent-child relationships after the logon.
6. **Look for persistence and impact:** Check relevant service/task/registry changes, file access, network connections and additional logons where telemetry exists.
7. **Treat log clearing carefully:** Event 1102 deserves context and escalation based on policy, but also consider approved maintenance, retention and forwarding failures.
8. **State the limits:** Missing event collection can hide steps in the timeline; don't infer absence from missing logs.

## 6. Analyst note template

- **Account / host / source:**
- **Time range + timezone:**
- **Relevant events and IDs:**
- **Observed sequence:**
- **Expected behaviour / baseline checked:**
- **Supporting evidence:**
- **Contradictory evidence / benign possibilities:**
- **Assessment + confidence:**
- **Next query / owner / escalation:**

## Practice prompt / Khud try karo

Synthetic dataset mein ek account ke liye many 4625 events, one 4624 and a 4688 process event milte hain. Decide karne se pehle kaunse 5 questions poochoge? Include logon type, host/source relationship, account baseline, process parent and telemetry gaps.
