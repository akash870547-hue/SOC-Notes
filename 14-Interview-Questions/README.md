# 14 · SOC Interview Question Vault

Use this for revision, then explain answers in your own words and add lab evidence where possible.

## SOC fundamentals

**Q1. Event vs alert vs incident?**  
An event is an observation; an alert is a rule/analytic output requiring review; an incident meets organizational response criteria. One incident may involve multiple alerts.

**Q2. What do you do when an alert fires?**  
Validate rule context and time range, identify entities, inspect raw events, correlate telemetry, assess impact/confidence, follow the playbook and leave an evidence-based handoff.

**Q3. Severity vs confidence?**  
Severity describes plausible impact/urgency; confidence describes how strongly evidence supports the assessment. Track separately.

## Networking

**Q4. What happens in DNS resolution?**  
A client asks a resolver for a name; it may answer from cache or query other servers. Correlate query, response, client, resolver, process context and timing.

**Q5. Why isn't every outbound connection command-and-control?**  
It may be normal application traffic or shared infrastructure. Review process, destination context, periodicity, bytes, DNS and corroborating events.

## Windows / endpoint

**Q6. Why are Sysmon 1 and Security 4688 useful?**  
They can provide process-creation evidence, but detail depends on collection/configuration. Review image, parent, command line, user and adjacent activity.

**Q7. What does Event ID 4625 show?**  
A failed logon. Interpret with account, source, status/substatus, logon type, baseline and any later success.

## Detection / response

**Q8. What makes a good detection?**  
Clear behavior, dependable telemetry, explainable logic, positive/negative tests, useful alert context, known false positives, owner and maintained response path.

**Q9. What is a false positive?**  
An alert that does not represent its intended behavior/risk. Document the evidence and tuning rationale.

**Q10. What is chain of custody?**  
A record of evidence collection, handling, transfer, storage and access.

**Q11. When would you isolate a host?**  
When evidence/risk meet policy and the action is authorized. Consider scope, business impact, evidence preservation and rollback.

**Q12. What if no evidence is found?**  
State scope, data sources, time range and result. Do not claim proof of absence if logs are missing or outside retention.

## Answer framework

For scenario questions use **Signal → Validate → Scope → Correlate → Assess → Act → Document**. Give evidence and reasoning, not only tool names.
