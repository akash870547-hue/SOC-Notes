# 10 · Threat Intelligence

Threat intelligence is useful when it turns observations into context that improves a decision. A feed hit is a lead, not a verdict.

## IOC record fields

| Field | Why capture it |
|---|---|
| Value + type | IP, domain, URL, hash, certificate, email or other observable |
| Source | Where it came from |
| First/last seen | Helps assess freshness |
| Confidence | Source and assessment reliability |
| Context | Associated behavior or infrastructure notes |
| Validity / expiry | Prevents stale indicators living forever |
| Environment sightings | Whether and where observed internally |
| Action taken | Enriched, monitored, blocked or dismissed—with rationale |

## Enrichment sequence

1. Preserve the original observable and case context.
2. Normalize carefully.
3. Check provenance, timestamps, confidence and source quality.
4. Correlate with internal telemetry and event time.
5. Consider shared hosting, CDNs, NAT and dynamic infrastructure.
6. Assign an outcome and expiration/review date.
7. Record why the indicator changed the decision.

## Safe training fixture

- **Observable:** example.invalid (reserved example domain)
- **Source:** synthetic training fixture
- **Confidence:** educational; not a real threat claim
- **Internal sighting:** synthetic DNS event in a lab
- **Disposition:** correlate with endpoint/process telemetry
- **Expiry:** review when lab data changes

## Common traps

- Treating an IP match as proof of compromise.
- Assuming a family label proves attribution.
- Failing to record enrichment time.
- Blocking shared infrastructure from one weak source.
- Publishing customer data or internal indicators without authorization.
