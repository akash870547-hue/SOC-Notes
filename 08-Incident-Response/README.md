# 08 · Incident Response

> Follow the organization's approved incident-response plan, decision authority and legal/privacy obligations. This checklist does not authorize disruptive changes.

## Response phases

- **Prepare:** roles, contacts, logging, backups, playbooks and exercises.
- **Detect & analyze:** validate evidence, classify, scope assets/identities and record uncertainty.
- **Contain:** choose authorized, proportionate actions; consider business impact and evidence preservation.
- **Eradicate:** remove verified cause/persistence and address exposed credentials or misconfiguration.
- **Recover:** restore from trusted sources, verify controls, monitor recurrence and obtain service-owner sign-off.
- **Learn:** timeline, detection gaps, improvements, owners and due dates.

## First-response checklist

1. Create/identify the case and preserve the initial alert context.
2. Confirm time window, affected entities, criticality and current impact.
3. Preserve relevant telemetry/evidence according to policy.
4. Expand scope with identity, endpoint, DNS, proxy, firewall and cloud audit events.
5. Escalate when impact, privilege, data exposure or uncertainty meets the playbook criteria.
6. Record each action with timestamp, operator, reason, approval and result.
7. Communicate facts separately from hypotheses; avoid unsupported attribution.
8. Document recovery checks, residual risk, evidence location and follow-up owners.

## Containment decision factors

| Factor | Ask |
|---|---|
| Scope | How many identities, hosts or services may be involved? |
| Impact | Is critical service, regulated data or privileged access at risk? |
| Persistence | Is activity continuing or could access recur? |
| Evidence | Could an action destroy volatile evidence or logs? |
| Trade-off | What is the operational cost and rollback path? |
| Authority | Who can approve the action? |

## Facts-first update

**Current assessment:** [confirmed facts only]  
**Scope:** [affected / potentially affected / unknown]  
**Actions taken:** [time, action, approver, result]  
**Open questions:** [specific gaps]  
**Next step / owner:** [action and checkpoint]  
**Confidence:** [level with reason]

Avoid guarantees that the evidence cannot support.
