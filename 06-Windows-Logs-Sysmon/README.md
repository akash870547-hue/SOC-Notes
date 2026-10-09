# 06 · Windows Logs & Sysmon

## The important distinction

Windows Security, System/Application, PowerShell and Sysmon provide different views. Collection depends on audit policy, configuration, event forwarding and retention. An absent event is not proof that an action did not occur.

## Quick event reference

| Channel / ID | Typical meaning | Useful context |
|---|---|---|
| Security 4624 | Successful logon | Account, logon type, source, target host |
| Security 4625 | Failed logon | Status/substatus, account, source, pattern |
| Security 4688 | Process created | Process, creator, command line when configured |
| Security 4720 | User account created | Subject, target, host and time |
| Security 4728 / 4732 | Member added to a security-enabled group | Subject, group and target member |
| Security 1102 | Security audit log cleared | Account, time and surrounding activity |
| Sysmon 1 | Process creation | Image, command line, parent, user, hash if configured |
| Sysmon 3 | Network connection | Process and endpoints; configuration-dependent |
| Sysmon 7 | Image loaded | Module and process; can be high-volume |
| Sysmon 10 | Process access | Source process, target process and access details |
| Sysmon 11 | File created | Process and target path |
| Sysmon 22 | DNS query | Process and queried name |

Check the event XML/details; availability and fields depend on policies, version and Sysmon configuration.

## Process-tree review

Capture timestamp/timezone, host, user/session, image path, signer/hash where available, full command line, parent chain, child activity, file writes, network activity, related alerts and expected admin/software context.

## Lab query: failed logons

~~~spl
index=lab earliest=-24h source="WinEventLog:Security" EventCode=4625
| stats count as failures dc(Computer) as hosts values(Status) as statuses by TargetUserName, IpAddress
| sort - failures
~~~

Inspect raw events before relying on the field names.

## Safe evidence hashing

PowerShell:
~~~powershell
Get-FileHash -Algorithm SHA256 -LiteralPath .\sample.evtx
~~~

Keep originals read-only where practical and record collector, time, source and working-copy location. A hash supports integrity checks; it does not prove maliciousness.

## Common traps

- Interpreting logon events without logon-type/context analysis.
- Assuming process events always include full command lines.
- Ignoring clock skew and ingestion delay.
- Treating a single event ID as a full timeline.
- Calling account creation malicious without surrounding evidence.
