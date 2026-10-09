# 11 · MITRE ATT&CK Field Guide

ATT&CK is a knowledge base of adversary tactics and techniques. Use it to describe observed behavior, guide hypotheses, identify detection opportunities and communicate coverage. It is not a severity scale or proof of actor identity.

## Terms

- **Tactic:** the adversary objective — the “why”.
- **Technique / sub-technique:** a behavior used to achieve that objective.
- **Telemetry:** evidence needed to observe the behavior.
- **Detection analytic:** a defined method for identifying the behavior.

## Evidence-first mapping

| Observation | Investigation before mapping |
|---|---|
| Encoded PowerShell command line | Review command line, parent process, user, script logs and follow-on behavior |
| Failures across many accounts | Review authentication pattern, source, identity architecture and any subsequent success |
| Account added to privileged group | Validate actor, target, approval, change window and neighboring events |

These are investigation paths, not automatic mappings. Confirm the current technique page and record rationale.

## Mapping checklist

- [ ] State the exact observed behavior.
- [ ] Identify telemetry that supports it.
- [ ] Select the narrowest mapping justified by evidence.
- [ ] Record confidence and alternatives.
- [ ] Link mapping to a detection test or response action.
- [ ] Revisit version changes for long-lived coverage documents.

## Coverage matrix

| Technique ID / name | Data source | Rule / analytic | Positive test | Known gap | Owner / review date |
|---|---|---|---|---|---|
| To validate | Endpoint / identity / network | Rule ID | Event fixture | Coverage caveat | Owner |

A technique name in a spreadsheet is not meaningful coverage unless relevant telemetry, tested logic and a usable response path exist.
