# 05 · Threat Hunting Playbook

Threat hunting is a hypothesis-led search for activity existing alerts may not have surfaced. Define scope, data requirements, repeatable pivots and a written outcome—even when nothing suspicious is found.

~~~mermaid
flowchart TD
 A[Write hypothesis] --> B[Check telemetry]
 B --> C[Define scope + baseline]
 C --> D[Query and pivot]
 D --> E[Validate candidates]
 E --> F{Evidence supports it?}
 F -- Yes --> G[Scope and escalate]
 F -- No --> H[Record result and gaps]
 G --> I[Improve detection / response]
 H --> I
 I --> A
~~~

## Hypothesis template

> If **[behavior]** occurs in **[environment]**, then **[observable evidence]** should appear in **[data sources]** during **[time window]**.

Example: If password spraying occurs, authentication logs may show failures across many accounts from a small number of sources, possibly followed by an unusual success.

## Hunt sequence

1. **Hypothesis:** explain the behavior and rationale.
2. **Scope:** systems, accounts, sources and time range; record missing coverage.
3. **Baseline:** normal volume, work hours, approved tools and service accounts.
4. **Search:** begin broadly, then narrow by stable fields.
5. **Pivot:** host → user → process → network → file / identity events.
6. **Validate:** inspect raw evidence and plausible alternatives.
7. **Outcome:** confirmed; suspicious but unconfirmed; no evidence in available data; or insufficient telemetry.
8. **Improve:** propose a detection, collection change, playbook update or follow-up.

## Example query (lab threshold only)

~~~spl
index=lab earliest=-24h EventCode=4625
| stats dc(TargetUserName) as accounts count as failures by IpAddress
| where accounts >= 8 AND failures >= 20
| sort - accounts
~~~

Tune thresholds to environment volume, identity architecture and expected failure patterns.

## Responsible negative result

“Queried authentication events for the stated 24-hour window. No candidate exceeded the defined threshold. This does not exclude lower-rate behavior, missing logs, alternate authentication paths or activity outside retention.”
