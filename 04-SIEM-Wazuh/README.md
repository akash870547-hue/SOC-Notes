# 04 · SIEM / Wazuh Field Guide

## What to recognize

A typical deployment includes endpoint **agents**, a **manager** for event analysis and rule evaluation, an **indexer** for searchable event storage, and a **dashboard** for investigation. Details vary by deployment and version.

## Alert-triage checklist

1. Record rule ID, level, timestamp, agent/host, source location and full event.
2. Determine whether it is a raw log, decoder output, correlated rule, FIM event or vulnerability/configuration finding.
3. Read the rule description and decoder path; confirm which fields matched.
4. Search the same host/user/process across a bounded window.
5. Compare with endpoint, network and identity data.
6. Validate rule changes in a test environment before use.

## Key concepts

| Concept | Practical meaning |
|---|---|
| Agent | Collects configured endpoint telemetry |
| Manager | Receives events and applies decoding/rules |
| Decoder | Parses raw log text into fields |
| Rule | Conditions and severity used to classify events |
| FIM | Detects configured file changes |
| Active response | Optional response integration with operational impact |
| Agent group | Helps apply shared configuration to endpoints |

## Rule quality questions

- Does the rule identify behavior or just a noisy keyword?
- Are expected admin tools and scheduled tasks considered?
- Is enough context retained for investigation?
- Is level proportionate to risk and confidence?
- Has it been tested against positive, negative and edge-case samples?
- Is there an owner, version note and rollback path?

## FIM investigation

A file-change event needs path, user/process if available, change type, host role and whether a legitimate update was expected. A changed file is not automatically malicious. Compare hashes and trusted deployment records where practical.

## Operational caution

Do not enable automated isolation, blocking, deletion or account disablement from an untested rule in production. Exercise active-response integrations in an isolated lab with rollback steps.
