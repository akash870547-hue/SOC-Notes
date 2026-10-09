# 12 · Use Cases & Sigma Rules

This section holds portable detection ideas, Sigma YAML examples, test plans and analyst response cards. Sigma rules still need conversion, field mapping and validation for the target SIEM.

## Use-case card

- **ID / title:**
- **Behavior to detect:**
- **Threat rationale:**
- **Required telemetry / fields:**
- **Logic + time window:**
- **Expected positives:**
- **Expected benign cases:**
- **False-positive controls:**
- **Triage pivots:**
- **ATT&CK mapping + rationale:**
- **Owner / version / review date:**

## Example rule

See [sigma/powershell-encoded-command.yml](sigma/powershell-encoded-command.yml). This is a starting point for test environments, not a validated production rule.

## Required validation set

1. A synthetic event intended to match.
2. A benign admin or software-management event.
3. Missing or null optional fields.
4. Case/argument-order/executable-path variations.
5. A time boundary or delayed-ingestion example.
6. Regression test after every rule change.

## Response card: suspicious script interpreter

1. Inspect command line, parent/child process chain, user and host criticality.
2. Check script logging, endpoint telemetry, file writes and network connections if collected.
3. Correlate with change tickets and approved admin activity.
4. Scope related alerts and identities within a bounded window.
5. Escalate if behavior, impact or uncertainty meets the playbook.
6. Save query, event IDs and disposition rationale.
