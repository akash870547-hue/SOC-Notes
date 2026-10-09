# 07 · Detection Engineering

A detection is a maintained analytic with a defined behavior, telemetry contract, rationale, tests, owner and response path—not just a query.

## Lifecycle

1. **Define:** behavior, risk, scope and success criteria.
2. **Check telemetry:** source, fields, timestamps, normalization and gaps.
3. **Implement:** explainable logic with bounded cost.
4. **Test:** positive, negative, edge-case and missing-field data.
5. **Tune:** expected admin activity, software updates, service accounts and baselines.
6. **Deploy:** reviewed, approved, versioned and reversible change.
7. **Observe:** alert volume, performance, false positives and analyst usefulness.
8. **Retire/revise:** document what changed and which coverage replaces it.

## Detection specification

- ID, title, owner and version
- Behavior and threat rationale
- Data sources, fields and prerequisites
- Logic and time window
- Positive / negative tests
- Alert context and triage steps
- False positives and tuning guidance
- Severity rationale
- ATT&CK mapping only where justified
- Deployment, rollback and review trigger

## Test matrix

| Input | Expected result | Validates |
|---|---|---|
| Positive lab event | Alert | Core logic |
| Similar benign admin event | No alert or defined lower-priority result | Discrimination |
| Missing optional field | Defined behavior | Robustness |
| Boundary time / delayed ingest | Matches documented behavior | Time handling |
| Duplicate event | Controlled count | Deduplication |
| Changed schema | Test fails visibly or review is triggered | Data contract |

## Quality questions

Can an analyst explain why the alert fired? Are exclusions narrow, owned and expiring? Is logic testable on a small fixture? Is the alert payload sufficient for triage? What adjacent telemetry compensates for likely evasions?

## Anti-patterns

Unowned global allowlists; broad keyword matching; unsupported ATT&CK mapping; raising severity to compensate for noise; and deploying untested logic with automated response attached.
