# 03 · SIEM / Splunk (SPL) Field Guide

> Replace sample index and field names with your lab's actual mappings. Field extraction varies by source and add-on.

## SPL query anatomy

~~~spl
index=lab earliest=-24h sourcetype="WinEventLog:Security" EventCode=4625
| stats count as failures dc(TargetUserName) as unique_users by Computer, IpAddress
| sort - failures
~~~

- **Base search:** limit index, time range and source first.
- **Filter:** narrow by known fields.
- **Transform:** use stats, timechart, table, dedup or related commands.
- **Validate:** inspect raw events and confirm field meaning.

## Useful patterns

### Failed logons by account and source

~~~spl
index=lab earliest=-24h EventCode=4625
| stats count as failures min(_time) as first_seen max(_time) as last_seen by TargetUserName, IpAddress, Computer
| sort - failures
~~~

### Trend over time

~~~spl
index=lab earliest=-24h sourcetype=dns
| timechart span=15m count by src_ip limit=10
~~~

### Process creation pivots

~~~spl
index=lab earliest=-4h EventCode=4688
| table _time Computer SubjectUserName NewProcessName ProcessCommandLine ParentProcessName
| sort 0 _time
~~~

Command-line fields can require a specific audit policy or endpoint source. Verify availability.

## Functions to learn

| Command | Why it matters |
|---|---|
| stats | Aggregate by dimensions |
| eval | Derive or normalize a field |
| where | Filter using expressions |
| timechart | Examine time patterns |
| transaction | Group events when justified; may be expensive |
| lookup | Enrich from a lookup table |
| rex | Extract fields with regular expressions |
| eventstats | Add aggregate context without collapsing rows |
| table | Present selected fields |

## Investigation habits

- Bound the time range, especially on large indexes.
- Inspect raw events after an aggregate surfaces an anomaly.
- Check timezone, ingestion delay, nulls, normalization and duplicate events.
- Avoid expensive joins/transactions until you know the data volume and need.
- Save the exact search and time range in the case notes.

## Common mistakes

Counting events as unique users; assuming all data has src_ip or EventCode; missing a success/failure sequence due to time-window boundaries; or using demo searches in production without validating scope and performance.
