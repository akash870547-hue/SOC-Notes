# Detecting Web DDoS — SOC L1 Field Notes

> Independent study sheet for **Web Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Web Security Monitoring · **Sheet:** 00

## Room focus

Request rate, source distribution, endpoint concentration and service health ko baseline ke against analyse karna.

## Concepts to carry into the room

- Volume can be distributed and mixed with legitimate traffic.
- Rate per source, route, status and time interval gives better context than total hits.
- Mitigation success should be measured against service health and collateral impact.

## Analyst lens

- Requests/sec, unique sources, URI distribution, latency/errors, upstream saturation and time series.
- CDN/load-balancer context and known campaign/marketing spikes.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

For a local/authorised access log, begin by validating its format and time zone, then group requests by interval, route, status and source as observed by the logging layer. Correlate suspicious requests with application/WAF and host telemetry. A status code alone does not prove exploit success.

## Common traps

- Treating a traffic spike as DDoS without service impact.
- Blocking all sources without assessing shared NAT or legitimate clients.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which service metric confirms impact?
- How does mitigation affect legitimate traffic?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
