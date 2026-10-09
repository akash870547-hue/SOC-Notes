# Detecting Web Attacks — SOC L1 Field Notes

> Independent study sheet for **Web Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Web Security Monitoring · **Sheet:** 00

## Room focus

Abnormal request sequences, input patterns and response behaviour ko defensive telemetry mein recognise karna.

## Concepts to carry into the room

- Single suspicious strings are not always an exploit attempt or success.
- Correlate web logs with app errors, WAF events and host/process telemetry.
- Avoid reproducing exploit payloads against systems outside the authorised lab.

## Analyst lens

- Request pattern, response, rate, session, application event and related host telemetry.
- Baseline and benign scanner/test traffic.

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

- Declaring exploitation based only on a request string.
- Testing a suspected payload against a production target.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What confirms impact beyond the request?
- Which benign workflow could look similar?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
