# 07 · SOC L1 Capstone Report Guide

**Best paired with:** the official [SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1), its SIEM triage rooms, and its capstone section.

This is an original report-writing guide. It does not include the answers, flags or the sequence of steps for any active capstone room.

## What a strong capstone submission should demonstrate

The important outcome isn't “I got the flag.” A SOC-ready portfolio entry should show that you can:
- State the scope and time window.
- Identify what evidence was actually available.
- Build a coherent timeline and connect related entities.
- Separate observed facts from your assessment.
- Check reasonable benign alternatives.
- Explain confidence, impact and telemetry gaps.
- Recommend an appropriate next action within the analyst's authority.
- Produce a handoff another analyst can continue.

## Evidence-to-conclusion table

| Finding | Evidence references | Alternative explanation considered | Confidence | Open question |
|---|---|---|---|---|
| | | | | |

Avoid claims such as “the attacker definitely exfiltrated data” unless the evidence establishes that. A suspicious connection might show an attempt or a transfer; distinguish the two.

## Timeline

| UTC time | Original time / timezone | Host / account / process | Source + event | Observation | Interpretation |
|---|---|---|---|---|---|
| | | | | | |

## Executive summary template

**Assessment:** [What the available evidence supports.]  
**Scope:** [Confirmed / potentially affected / unknown.]  
**Impact:** [Business or technical impact supported by evidence.]  
**Actions:** [Approved actions, owner, time and result.]  
**Confidence:** [Level and brief reason.]  
**Gaps:** [Missing telemetry or facts not established.]  
**Next step:** [Action, owner and checkpoint.]

## Example: how to phrase uncertainty

Instead of: “This IP proves the endpoint is compromised.”

Prefer: “The endpoint attempted a connection to the destination during the observed window. The connection alone does not establish compromise. The next validation is to correlate DNS, process telemetry, proxy/firewall action and related alerts, then assess the destination's relevance and expected business use.”

## Portfolio-safe publishing checklist

- [ ] Do not include flags, room answers or active-room walkthrough steps.
- [ ] No certification/exam details.
- [ ] No private room dashboard data, credentials or tokens.
- [ ] No customer/employer logs or personal data.
- [ ] Use synthetic examples for public illustrations.
- [ ] State that the note is independent and link to the official room/path.
- [ ] Make the finding understandable without disclosing protected challenge content.

## Hinglish takeaway

Capstone ko professional case report mein convert karo: kya dekha, kaise verify kiya, kya uncertain hai, aur next analyst ko kya karna chahiye.
