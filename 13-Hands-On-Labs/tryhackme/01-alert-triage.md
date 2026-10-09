# 01 · SOC Alert Triage — Field Notes

**Best paired with:** the official [SOC L1 Alert Triage](https://tryhackme.com/room/socl1alerttriage), [SOC Fundamentals](https://tryhackme.com/room/socfundamentals), and [Junior Security Analyst Intro](https://tryhackme.com/room/jrsecanalystintrouxo) rooms.

This is an original workflow guide, not a room walkthrough. Room-specific alerts, answers and flags are intentionally not reproduced.

## The analyst mindset / Sochne ka tareeqa

Alert ko starting point samjho, final verdict nahi. A detection rule has matched something; now your job is to understand *what the evidence says*, what else you need, and what action your role is authorized to take.

## A reusable seven-step workflow

1. **Read the alert.** Capture its name, rule ID, severity, source, event time, alert time, affected entity and current status.
2. **Take ownership properly.** Follow the team's process for assignment/status, check if another analyst already owns it, and keep changes auditable.
3. **Understand the behaviour.** What action was detected—authentication, process creation, network connection, file change or something else?
4. **Scope the entity.** Identify the host/account/IP/application and its business role. Check whether the activity fits the normal baseline.
5. **Correlate context.** Inspect relevant events before and after the trigger. Look across endpoint, identity, DNS, firewall, proxy or cloud audit telemetry when available.
6. **Assess confidence and impact.** Keep severity, confidence and priority separate. Record both supporting and contradictory evidence.
7. **Document and route.** Close only with defensible reasoning, or escalate using the approved playbook. Include a useful next step and owner.

## Triage decision vocabulary

| Outcome | Use when | What to document |
|---|---|---|
| Benign / false positive | Evidence supports an expected or irrelevant activity for this analytic | Expected cause and why evidence fits |
| Suspicious / unconfirmed | Behaviour is concerning, but available evidence is incomplete | Hypothesis, supporting clues, missing data |
| Confirmed incident / true positive | Evidence supports the defined malicious behaviour under your process | Evidence references, scope, impact and escalation |
| Insufficient telemetry | Required sources/fields are missing or cannot establish the claim | What was checked and what needs collection |

Labels may differ by organization. Follow the local incident taxonomy.

## The 5Ws + evidence

- **What:** What behaviour was recorded?
- **When:** Event time, alert time, timezone and ingestion delay?
- **Where:** Which host, identity, app, network segment or cloud resource?
- **Who:** Which account/process/entity initiated the activity, based on logs?
- **Why:** Which evidence supports a legitimate explanation or a threat hypothesis?

Add two more fields for a strong note: **How confident are we, and what evidence is missing?**

## Example: synthetic login anomaly

Suppose a synthetic dataset contains several authentication failures and a later successful login. Do not automatically label this as compromise. Ask:

- Are the same account, source and host involved?
- Is the pattern unusual for this user and time of day?
- Do logon type, source network, VPN/NAT or service-account behaviour explain it?
- Are there later privilege or process events?
- Is the data complete for the entire time window?

The threshold and final conclusion depend on context, not one universal number.

## Reusable case comment

~~~text
Summary:
Time window + timezone:
Entities in scope:
Observed facts:
Evidence checked:
Supporting evidence:
Benign alternatives considered:
Assessment + confidence:
Data gaps:
Action taken / approval:
Next step + owner:
~~~

## What good looks like

Another analyst should be able to understand why you made the decision, reproduce the key searches and continue from the next action. “Looks malicious” and “closed” are not enough.
