# Network Discovery Detection — SOC L1 Field Notes

> Independent study sheet for **Network Security Monitoring**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Network Security Monitoring · **Sheet:** 00

## Room focus

Scanning/discovery hypotheses ko rate, target diversity, response patterns and baseline se assess karna.

## Concepts to carry into the room

- Many destinations/ports in a short window can be a discovery clue, not proof alone.
- Asset inventory and vulnerability scanners can explain legitimate sweeps.
- Use source identity and change-window context before escalation.

## Analyst lens

- Source host/user, unique destinations/ports, timestamps, response patterns and known scanner inventory.
- Comparison with normal baseline.

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

- Assuming high fan-out always means an attacker.
- Ignoring NAT/shared scanners and scheduled inventory jobs.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- What makes the activity abnormal for this network?
- What benign source could produce the same pattern?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
