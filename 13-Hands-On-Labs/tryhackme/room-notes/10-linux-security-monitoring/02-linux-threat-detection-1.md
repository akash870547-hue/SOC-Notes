# Linux Threat Detection 1 — SOC L1 Deep-Dive Notes

> **Independent analyst companion · Linux Security Monitoring**  
> Technical concepts, investigation method, validation logic, safe tooling and reporting practice. This is not an official TryHackMe page and contains no room answers, flags or active-room solution steps.

## 1. Learning objective

Linux authentication and system events se suspicious patterns ko normal administration se separate karna.

After this topic, you should be able to explain the data source, perform a small reproducible investigation, separate observed facts from inference, consider a benign alternative, state important visibility limits and recommend a proportionate next action.

## 2. Mental model

- Authentication files, journald, auditd, service logs and EDR provide different evidence types.
- Distribution and logging configuration determine file paths, fields, retention and forwarding.
- SSH, sudo, new users/keys, service changes and outbound process activity must be compared with legitimate automation.
- A missing path or empty query is a visibility observation, not proof that activity did not occur.

## 3. Room-specific analyst lens

**Focus:** Linux authentication and system events se suspicious patterns ko normal administration se separate karna.

**Technical angle:** Interpret failed/successful sessions with source, account, bastion/NAT, SSH key/MFA and events after authentication.

For your own lab session, answer these questions with evidence rather than memory:

- Which artefact most directly supports the hypothesis, and which field matters?
- Which benign workflow could produce a similar pattern?
- What source or field is missing, stale or ambiguous?
- What evidence would cause you to lower or raise confidence?

## 4. Investigation workflow

1. Record hostname, distribution, timezone, query time and privilege context; avoid changing state during evidence collection.
2. Confirm whether logs are in journald, classic files, auditd or a central forwarder.
3. Search a bounded interval for authentication, privilege and service-change records.
4. Correlate UID/user, source IP, process ID, command, unit name, file metadata and network endpoints.
5. Check rotation, forwarder status, containers and approved automation; state the limitations explicitly.

## 5. Signals, meaning and validation

| Observation pattern | Why it may matter | Validate before concluding |
|---|---|---|
| Auth anomaly | Repeated failures or unusual successful login. | Check bastion, NAT, SSH keys/MFA, user schedule, source and following commands. |
| Privilege/service change | sudo, new unit/user/key or scheduler change. | Correlate audit/process logs, package manager, change ticket and admin ownership. |
| Possible persistence | New scheduled job or startup artifact behaves unexpectedly. | Preserve owner, hash, mode and timestamps; compare deployment baseline. |

## 6. Tooling and analyst data

Read-only examples: `journalctl --since '24 hours ago'`; on systems that use it, `grep -E 'Failed password|Accepted password' /var/log/auth.log`. Confirm distro, permissions and source first; no output is not an all-clear.

Read-only local examples:
```bash
journalctl --since '24 hours ago'
# Only on systems that use this path:
grep -E 'Failed password|Accepted password' /var/log/auth.log
```

**Reproducibility rule:** record exact query/filter, source or data view, time range/timezone, tool/version and raw event/frame/document references. Counts and dashboards help prioritise; raw evidence supports the conclusion. Use only platform-assigned labs or systems you own/are authorised to test.

## 7. False-positive and blind-spot review

- Assuming every distribution has `/var/log/auth.log`.
- Executing suspicious binaries or deleting artefacts during triage.
- Ignoring cloud-init, containers, configuration management and ephemeral hosts.

Before closure, check wrong timezone, delayed ingestion, incomplete retention, non-unique join keys, duplicate records, stale enrichment, parser/schema changes and alternative legitimate workflows. An empty search result means only that the query returned no matches under its present assumptions.

## 8. Independent practice drill

On a Linux VM you own, compare journal output with the configured auth log and note field/time/retention differences. Keep the test read-only.

**Room-specific task:** Interpret failed/successful sessions with source, account, bastion/NAT, SSH key/MFA and events after authentication.

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

- [journalctl manual](https://man7.org/linux/man-pages/man1/journalctl.1.html)
- [systemctl manual](https://man7.org/linux/man-pages/man1/systemctl.1.html)
- [auditd manual](https://man7.org/linux/man-pages/man8/auditd.8.html)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)

- [Official TryHackMe SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1)
- [TryHackMe Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) — check the current policy and room status before publishing anything room-specific.

---

**Publishing boundary:** TryHackMe's current policy prohibits publishing flags, answers, solutions and step-by-step walkthroughs for active content; certification/exam content has separate permanent restrictions. Keep public notes conceptual and spoiler-free, and record your own observations separately.
