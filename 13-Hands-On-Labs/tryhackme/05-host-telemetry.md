# 05 · Host Telemetry — Windows, Linux and EDR

**Best paired with:** [Introduction to EDR](https://tryhackme.com/room/introductiontoedrs), [Windows Logging for SOC](https://tryhackme.com/room/windowsloggingforsoc), [Windows Threat Detection 1](https://tryhackme.com/room/windowsthreatdetection1), [Windows Threat Detection 2](https://tryhackme.com/room/windowsthreatdetection2), [Windows Threat Detection 3](https://tryhackme.com/room/windowsthreatdetection3), [Linux Logging for SOC](https://tryhackme.com/room/linuxloggingforsoc), and [Linux Threat Detection 1–3](https://tryhackme.com/room/linuxthreatdetection1).

## Windows: build a timeline, not an event-ID bingo card

| Event / source | Possible investigative value | Context still needed |
|---|---|---|
| Security 4624 | Successful logon | Logon type, account, source, host and baseline |
| Security 4625 | Failed logon | Status/substatus, repetition, target and source |
| Security 4672 | Special privileges in a logon context | Whether the account/session was expected to be privileged |
| Security 4688 | Process creation | Image, command line if collected, user, parent lineage |
| Security 1102 | Security audit log cleared | Who, when, change context, forwarding/retention status |
| Sysmon 1 | Process creation | Parent, user, command line, hashes where configured |
| Sysmon 3 | Network connection | Source process and actual connection context when collected |
| Sysmon 11 | File creation | Path, process and expected software activity |

These are common references, not a guarantee that every system collects every event. Validate the raw event schema and audit/Sysmon configuration.

## Linux: where to start

Common locations vary by distribution:
- Debian/Ubuntu commonly use /var/log/auth.log for authentication-related events.
- RHEL-like systems commonly use /var/log/secure.
- Systemd-based environments may expose logs through journalctl.
- Auditd records depend on rules being installed and enabled.

Read logs according to your lab permissions. Avoid assuming one file or field exists on every host.

### Defensive commands for an authorised lab

~~~bash
# Inspect recent authentication-related messages
sudo journalctl --since "24 hours ago" --no-pager

# Search a local auth log when present
grep -Ei "failed|invalid user|accepted|session opened" /var/log/auth.log

# Record a hash for a collected evidence file
sha256sum ./collected-log-copy.txt
~~~

Adapt commands to the system and collection policy. A query match is only a lead; confirm timestamps, usernames, source addresses and the event sequence.

## Process lineage questions

When EDR/Sysmon shows a process:
- Who launched it, and under which logon/session?
- Is the parent-child relationship expected for this application?
- What was the full command line, if available?
- Were new files, registry/service/task changes or outbound connections observed?
- Is there a change ticket, software deployment or approved admin workflow?
- Do independent sources corroborate the activity?

A browser/document spawning a shell deserves investigation, but the relationship alone does not prove malware.

## Timeline template

| Time (UTC) | Host | User / session | Event source + ID | Observation | Evidence reference | Confidence |
|---|---|---|---|---|---|---|
| | | | | | | |

Keep original timestamps and timezone information alongside normalised UTC. Note missing log sources and potential clock skew.

## EDR response considerations

Containment/isolation, process termination, deletion, credential changes and blocking can be disruptive. Use the authorised incident playbook, involve the relevant owner, consider evidence preservation, and record approval, time, outcome and rollback where applicable.

## Hinglish takeaway

Ek Event ID ko akela dekh kar verdict mat do. User, host, time, process chain aur network context ko connect karna hi actual host investigation hai.
