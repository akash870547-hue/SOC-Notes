# Snort — SOC L1 Field Notes

> Independent study sheet for **Network Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Network Security Monitoring · **Sheet:** 00

## Room focus

Snort alert/rule semantics and packet-level evidence ko interpret karna.

## Concepts to carry into the room

- Rule action, protocol, address/port constraints and content options shape matches.
- Alert output must be tied to the relevant packet/capture and rule revision.
- Validation should include benign cases and environment-specific baselines.

## Analyst lens

- Rule SID/revision, timestamp, endpoints, alert text and underlying packet evidence.
- Test result and tuning rationale.

## A practical way to organise your own investigation

Use this as a general analyst workflow, not as a sequence of room-specific answers:

1. **Scope:** note the authorised lab/asset, time range, relevant identity or network entities, and the data source being examined.
2. **Observe:** record raw evidence and query/filter details before summarising it.
3. **Correlate:** connect records only when identifiers, timestamps and context support the relationship.
4. **Challenge the hypothesis:** seek a benign explanation and note what evidence is missing.
5. **Decide and communicate:** state the verdict with confidence, evidence, impact, next action and escalation owner.

## Tooling / reference notes

Wireshark display-filter reminders (apply to an authorised capture):

```text
dns
http.request
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

A command-line conversation summary can be generated with `tshark -r <authorized-capture.pcap> -q -z conv,tcp`. Preserve the original capture and note capture-point/packet-loss limitations.

## Common traps

- Blindly copying rules into production.
- Tuning away alerts without measuring detection loss.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Which rule condition actually matched?
- How can an authorised test validate the rule safely?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
