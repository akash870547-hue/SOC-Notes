<!--
  SOC-Notes | Analyst Field Manual
  Built for learning, lab work, and responsible defensive security.
-->

<div align="center">

<img src="assets/soc-console.svg" alt="SOC-Notes analyst field manual" width="100%"/>

# SOC NOTES // ANALYST FIELD MANUAL

**From raw telemetry to a defensible decision.**

[**Start Here**](#-start-here) · [**Learning Roadmap**](#-the-analyst-path) · [**Detection Library**](#-detection--investigation-workbench) · [**Hands-on Labs**](#-lab-queue)

![Focus](https://img.shields.io/badge/FOCUS-SOC%20%7C%20Detection%20%7C%20DFIR-111827?style=flat-square)
![Approach](https://img.shields.io/badge/APPROACH-Understand%20%7C%20Validate%20%7C%20Document-0d9488?style=flat-square)
![Safety](https://img.shields.io/badge/LABS-Synthetic%20%26%20Authorized-166534?style=flat-square)

</div>

---

> **Field rule:** A log line is an observation, not a verdict. Verify the context, correlate the evidence, record uncertainty, and explain the decision.

## ◈ Start here

This repository is a growing, practical knowledge base for Security Operations Center (SOC) analysis. It is organized like an analyst's field manual: concise concepts, triage workflows, telemetry references, detection logic, investigation checklists, and reproducible labs.

- **New to SOC?** Follow the [Analyst Path](#-the-analyst-path) in order.
- **Working an alert?** Use the [Triage Card](#-the-60-second-triage-card), then move to the relevant topic.
- **Writing a detection?** Start in [Detection Engineering](07-Detection-Engineering/README.md) and [Sigma Rules](12-Use-Cases-Sigma-Rules/README.md).
- **Need investigation steps?** Open [Incident Response](08-Incident-Response/README.md) or [DFIR](09-DFIR/README.md).
- **Preparing for interviews?** Use the [Interview Vault](14-Interview-Questions/README.md).

## ◈ The analyst path

<img src="assets/incident-lifecycle.svg" alt="SOC investigation lifecycle" width="100%"/>

| Phase | Analyst question | Start here |
|---|---|---|
| 01 · Orient | What does a SOC do, and what is an alert? | [SOC Fundamentals](01-SOC-Fundamentals/README.md) |
| 02 · Read telemetry | What can network and host logs prove? | [Network Security](02-Network-Security/README.md) · [Windows & Sysmon](06-Windows-Logs-Sysmon/README.md) |
| 03 · Query | How do I find the signal in the noise? | [Splunk](03-SIEM-Splunk/README.md) · [Wazuh](04-SIEM-Wazuh/README.md) |
| 04 · Investigate | What happened before and after the alert? | [Threat Hunting](05-Threat-Hunting/README.md) · [MITRE ATT&CK](11-MITRE-ATTACK/README.md) |
| 05 · Detect | Can this behavior be detected reliably? | [Detection Engineering](07-Detection-Engineering/README.md) · [Use Cases & Sigma](12-Use-Cases-Sigma-Rules/README.md) |
| 06 · Respond | What should be contained, preserved, and communicated? | [Incident Response](08-Incident-Response/README.md) · [DFIR](09-DFIR/README.md) |
| 07 · Improve | What did the case teach us? | [Labs](13-Hands-On-Labs/README.md) · [Interview Vault](14-Interview-Questions/README.md) |

## ◈ The 60-second triage card

Use this as a **memory aid**, not as a replacement for your organization's runbook.

1. **Validate the alert:** identify the rule, severity, triggering fields, time range, and data source.
2. **Scope the entity:** host, user, IP, process, file, cloud identity, or application; confirm asset criticality and business context.
3. **Build the timeline:** inspect the lead-up and follow-on activity; normalize timestamps and account for time zones.
4. **Correlate:** compare endpoint, authentication, DNS, proxy, firewall, identity, and cloud events where available.
5. **Assess impact:** identify affected assets, privileges, persistence indicators, lateral movement, and data-access evidence.
6. **Act within authority:** follow the incident playbook; preserve evidence before destructive remediation where practical.
7. **Write the handoff:** state what is known, what is unknown, evidence links, actions taken, owner, and next checkpoint.

### A clean analyst note answers

- **What triggered?** Exact rule, event IDs, and time window.
- **Why does it matter?** Relevant behavior and likely impact.
- **What confirms or weakens it?** Supporting and contradictory evidence.
- **What happened next?** Correlated timeline and scope.
- **What do we recommend?** Action, owner, priority, and rationale.

## ◈ Detection & investigation workbench

| Workbench | What belongs here |
|---|---|
| [Splunk / SPL](03-SIEM-Splunk/README.md) | Search structure, time windows, field extraction, stats, joins and pivots |
| [Wazuh](04-SIEM-Wazuh/README.md) | Agents, decoders, rules, alert triage and integrations |
| [Windows & Sysmon](06-Windows-Logs-Sysmon/README.md) | Event IDs, process lineage, authentication and endpoint telemetry |
| [Threat Hunting](05-Threat-Hunting/README.md) | Hypotheses, pivots, baselines, hunt write-ups and negative findings |
| [Detection Engineering](07-Detection-Engineering/README.md) | Detection lifecycle, tuning, false positives, testing and coverage |
| [Sigma & Use Cases](12-Use-Cases-Sigma-Rules/README.md) | Portable rules, use-case cards, test data and ATT&CK mapping |
| [Threat Intelligence](10-Threat-Intelligence/README.md) | IOC handling, enrichment, confidence, freshness and context |

## ◈ Lab queue

All examples should use **synthetic telemetry, intentionally vulnerable local labs, or systems you are explicitly authorized to test**. Do not paste real credentials, personal data, internal hostnames, or customer logs into public notes.

| Lab | Objective | Deliverable |
|---|---|---|
| L-01 · Login anomaly | Find repeated failures followed by success | Timeline + triage note |
| L-02 · Process lineage | Explain a suspicious parent-child process chain | Process tree + confidence rating |
| L-03 · DNS pivot | Correlate endpoint, DNS, and network events | Entity map + scoped findings |
| L-04 · Rule validation | Test a Sigma-style detection against benign and positive samples | Test matrix + tuning notes |
| L-05 · Incident report | Build a facts-only incident timeline | Executive summary + technical appendix |

See [Hands-On Labs](13-Hands-On-Labs/README.md) for scenario cards and expected outputs.

## ◈ Working principles

- **Evidence before conclusion.** Separate facts, hypotheses, and assumptions.
- **Context beats isolated indicators.** An IOC match alone is not proof of compromise.
- **Time matters.** Record timezone, clock skew, source, and collection time.
- **Preserve integrity.** Hash relevant acquired files and document handling.
- **Detection is a lifecycle.** Test, tune, version, monitor, and retire rules deliberately.
- **Write for the next analyst.** Clear handoffs reduce repeated work and missed signals.
- **Stay authorized.** Hands-on activity stays in owned or explicitly permitted environments.

## ◈ Repository map

| Directory | Purpose |
|---|---|
| [01-SOC-Fundamentals](01-SOC-Fundamentals/README.md) | SOC roles, alert lifecycle, severity and analyst workflow |
| [02-Network-Security](02-Network-Security/README.md) | TCP/IP, DNS, HTTP(S), firewall, proxy and flow analysis |
| [03-SIEM-Splunk](03-SIEM-Splunk/README.md) | SPL patterns, searches, pivots and troubleshooting |
| [04-SIEM-Wazuh](04-SIEM-Wazuh/README.md) | Wazuh components, rules, alerts and triage |
| [05-Threat-Hunting](05-Threat-Hunting/README.md) | Hypothesis-driven hunts, baselines and evidence pivots |
| [06-Windows-Logs-Sysmon](06-Windows-Logs-Sysmon/README.md) | Windows Security logs, Sysmon and process analysis |
| [07-Detection-Engineering](07-Detection-Engineering/README.md) | Rule lifecycle, quality, testing and tuning |
| [08-Incident-Response](08-Incident-Response/README.md) | Containment, escalation, communications and recovery |
| [09-DFIR](09-DFIR/README.md) | Evidence, timeline, triage, acquisition and reporting |
| [10-Threat-Intelligence](10-Threat-Intelligence/README.md) | IOC enrichment, confidence, freshness and context |
| [11-MITRE-ATTACK](11-MITRE-ATTACK/README.md) | Tactics, techniques, evidence and coverage mapping |
| [12-Use-Cases-Sigma-Rules](12-Use-Cases-Sigma-Rules/README.md) | Detection use cases, Sigma examples and test plans |
| [13-Hands-On-Labs](13-Hands-On-Labs/README.md) | Safe scenario-based practice |
| [14-Interview-Questions](14-Interview-Questions/README.md) | SOC L1/L2, SIEM, networking and DFIR revision |
| [resources](resources/README.md) | Glossary, references, templates and further reading |

## ◈ Note template

For new topic notes, use this small repeatable structure:

> **Concept → Why it matters → Data source → Investigation steps → Example query/rule → False positives → Validation → References**

For incident or hunt write-ups, add: **scope, time range, assumptions, evidence IDs, findings, confidence, limitations, next action, and reviewer**.

## ◈ Contribution / note drop

Bring raw notes, screenshots, PDFs, commands, class notes, or rough explanations. They can be turned into structured Markdown with diagrams, examples, cross-links, glossary entries, and lab exercises. Sensitive logs and secrets should be sanitized before sharing.

**Status:** living field manual — content is meant to improve as labs are tested and notes are added.

---

<div align="center">

**SOC-NOTES // COLLECT → CORRELATE → VALIDATE → DOCUMENT**

<sub>Defensive learning resource · Examples are illustrative unless explicitly labeled as tested.</sub>

</div>
