# Wireshark: The Basics — SOC L1 Field Notes

> Independent study sheet for **Network Traffic Analysis**. This page is designed to help you understand the defensive skill and document your own observations; it is **not** an answer key or a room walkthrough.

**Official path:** [TryHackMe SOC Level 1](https://tryhackme.com/path/outline/soclevel1) · **Module:** Network Traffic Analysis · **Sheet:** 00

## Room focus

Display filters, packet details, conversations and stream context ko samajhna.

## Concepts to carry into the room

- Capture filters and display filters operate at different stages.
- A display filter narrows viewing; it does not modify the source capture.
- Follow-stream views are helpful but should be validated against packet records.

## Analyst lens

- Filter used, packet numbers, stream/conversation endpoints and protocol fields.
- PCAP source, capture time and limitations.

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

- Confusing display filters with capture filters.
- Reporting only a screenshot without a reproducible filter.

## Personal evidence worksheet

Fill this with **your own observations** from an authorised session. Avoid publishing flags, answers, or restricted room-specific details.

| UTC timestamp / range | Source or artefact | Observed fact (not interpretation) | Interpretation / confidence | Next pivot |
|---|---|---|---|---|
| _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ | _Fill in_ |

## Write-up prompts

- Can you explain each filter term?
- What evidence exists outside the selected packet?

When you finish, write a short report with: **summary**, **scope**, **key evidence**, **verdict and confidence**, **benign alternatives considered**, **impact**, **response or escalation**, and **limitations / next steps**. Every material conclusion should point back to a source or observation.

---

**Publishing note:** TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing answers, flags, solutions and step-by-step walkthroughs for active content. Keep this public sheet conceptual and spoiler-free; keep your own private learning notes separate, and only publish room-specific write-ups when the room is marked retired and the policy permits it.
