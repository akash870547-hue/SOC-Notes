# 13 · Hands-On Labs

Use synthetic events or systems you own / are explicitly authorized to test. The objective is to produce analyst artifacts, not merely a query with results.

## Lab 01 · Login anomaly

**Scenario:** A synthetic dataset contains repeated failed logons followed by a successful logon. Decide whether it looks like user error, automation or suspicious activity.

**Tasks**
1. Establish baseline failed logons by user and source.
2. Find accounts with failures across multiple hosts/sources.
3. Check whether success follows the failures in the same window.
4. Inspect logon type, status codes, VPN/NAT behavior and asset context.
5. Produce a timeline and list evidence that would change your assessment.

**Illustrative SPL**
~~~spl
index=lab earliest=-24h EventCode IN (4624,4625)
| stats count values(EventCode) as event_types min(_time) as first_seen max(_time) as last_seen by TargetUserName, IpAddress, Computer
| sort - count
~~~

Validate the syntax and field availability in your environment.

**Deliverable:** triage note, query, gaps, outcome (confirmed / suspicious / benign / insufficient telemetry).

## Lab 02 · Process lineage

Use synthetic process events to explain parent → child relationships. Identify user, command line, executable path and surrounding activity; document benign alternatives.

## Lab 03 · DNS pivot

Correlate a synthetic DNS event with endpoint process and network metadata. Distinguish ordinary repeated queries from evidence that supports a suspicious hypothesis.

## Lab 04 · Rule validation

Use [the Sigma fixture](../12-Use-Cases-Sigma-Rules/sigma/powershell-encoded-command.yml) to create positive, benign and edge-case fixtures. Document limits and tuning before any production use.

## Lab 05 · Incident report

Produce an evidence-linked timeline and factual executive summary using the [incident report template](../resources/templates/incident-report.md).

## Acceptance checklist

- [ ] Scope and time range stated
- [ ] Queries and steps are reproducible
- [ ] Raw evidence inspected
- [ ] Facts and hypotheses separated
- [ ] Benign alternatives considered
- [ ] Limitations stated
- [ ] Another analyst can continue from the output
