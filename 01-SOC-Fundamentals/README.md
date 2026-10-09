# 01 · SOC Fundamentals

> **Mission:** turn security telemetry into a timely, evidence-backed decision.

## SOC in one view

A Security Operations Center monitors an environment, validates suspicious activity, investigates scope and impact, coordinates response, and improves detections.

~~~mermaid
flowchart LR
  A[Telemetry sources] --> B[Collection / SIEM]
  B --> C[Rule or analytic]
  C --> D[Alert triage]
  D --> E{Evidence supports risk?}
  E -- No / benign --> F[Document and tune]
  E -- Yes / uncertain high impact --> G[Escalate / investigate]
  G --> H[Containment via approved playbook]
  H --> I[Recovery and lessons learned]
  I --> F
~~~

## Core vocabulary

| Term | Meaning | Analyst caution |
|---|---|---|
| Event | A recorded observation | An event alone may be ordinary |
| Alert | A rule or analytic says activity deserves review | Severity is not proof of compromise |
| Incident | Events meeting the organization's incident criteria | Use the approved classification process |
| IOC | Observable such as a hash, domain, IP or filename | Indicators can be stale or shared |
| TTP | Adversary tactic, technique or procedure | Behavior is often more durable than one IOC |
| False positive | Alert does not represent its intended behavior/risk | Record why, not just “FP” |
| True positive | Alert correctly identified the defined behavior | A TP is not automatically a confirmed breach |

## Severity is not confidence

- **Severity / impact:** how bad the plausible outcome could be.
- **Confidence:** how strongly evidence supports the assessment.
- **Priority:** urgency, often combining impact, confidence, asset criticality and business context.

## L1 triage sequence

1. Read the rule, trigger fields, source, timestamp and time window.
2. Identify the host, user, IP, process, identity or application.
3. Validate asset criticality and expected behavior.
4. Review nearby events; normalize time zones and ingestion delay.
5. Correlate endpoint, identity, DNS, proxy, firewall and cloud events where available.
6. Close with rationale, investigate further or escalate under the playbook.
7. Leave a reproducible handoff: query, scope, evidence, gaps, actions, owner and next step.

## Handoff mini-template

- **Alert / case ID:**
- **Time window + timezone:**
- **Entities in scope:**
- **Observed facts:**
- **Supporting / contradictory evidence:**
- **Confidence and impact:**
- **Actions taken:**
- **Open questions / next owner:**

## Knowledge check

**An alert says “suspicious PowerShell.” Is the host compromised?** Not by itself. Establish the parent process, command line, user, script context, child activity, network behavior and business purpose before concluding.
