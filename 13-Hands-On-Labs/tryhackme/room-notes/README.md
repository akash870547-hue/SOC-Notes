# TryHackMe SOC Level 1 — Room-by-Room Field Notes

> **68 individual study sheets** mapped to the named rooms currently listed in the [official SOC Level 1 path](https://tryhackme.com/path/outline/soclevel1). This is an independent, spoiler-free learning companion, not official TryHackMe material.

## Important publishing boundary

TryHackMe's [Acceptable Use Policy](https://tryhackme.com/legal/acceptable-use-policy) prohibits publishing flags, answers, solutions, exploit code written for a specific active room, or step-by-step walkthroughs for active content; certification/exam material is protected permanently. These pages therefore cover transferable concepts, defensive analysis methods, tool reminders and blank evidence worksheets—not room answers or solution sequences. Room status can change, so check current policy and whether a room is marked retired before publishing any room-specific write-up.

## How to use the collection

1. Use the official path to open the room and complete the lab yourself.
2. Read the matching sheet for the underlying concept and relevant defensive workflow.
3. Fill the blank evidence worksheet from your own observations; preserve sources, UTC times, queries and uncertainty.
4. Finish with a concise analyst report and note what evidence would change your conclusion.

## Room sheets

### 01 · Blue Team Introduction

SOC ka role, responsibilities, handoffs aur user/system attack surfaces ko defensive lens se samajhna.

- [Junior Security Analyst Intro](01-blue-team-introduction/01-junior-security-analyst-intro.md) — L1 analyst ke daily work, queue ownership, escalation aur clear communication ko map karo.
- [SOC Role in Blue Team](01-blue-team-introduction/02-soc-role-in-blue-team.md) — SOC ko detection, investigation, response, intelligence feedback aur cross-team coordination ke beech position karo.
- [Humans as Attack Vectors](01-blue-team-introduction/03-humans-as-attack-vectors.md) — Human-focused threats ko blame ke bajay observable behaviour, context aur usable controls se assess karo.
- [Systems as Attack Vectors](01-blue-team-introduction/04-systems-as-attack-vectors.md) — Misconfiguration, exposed services, weak identity controls and unpatched software ko defensive risk ke roop mein dekhna.

### 02 · SOC Team Internals

Alert triage, case writing, repeatable workbooks aur operational metrics ko evidence-based banana.

- [SOC L1 Alert Triage](02-soc-team-internals/01-soc-l1-alert-triage.md) — Triage state, priority, evidence and decision rationale ko consistent tareeke se record karna.
- [SOC L1 Alert Reporting](02-soc-team-internals/02-soc-l1-alert-reporting.md) — Concise, reproducible case notes aur evidence-based handoffs likhna.
- [SOC Workbooks and Lookups](02-soc-team-internals/03-soc-workbooks-and-lookups.md) — Repeatable enrichment and investigation references ko transparent, maintained aur scoped rakhna.
- [SOC Metrics and Objectives](02-soc-team-internals/04-soc-metrics-and-objectives.md) — Detection and response metrics ko quality, workload and business outcomes ke saath interpret karna.

### 03 · Core SOC Solutions

SIEM, EDR aur SOAR ke collection, investigation, response aur automation boundaries ko compare karna.

- [Introduction to EDR](03-core-soc-solutions/01-introduction-to-edr.md) — Endpoint telemetry, process ancestry and authorised response actions ke concepts ko connect karna.
- [Introduction to SIEM](03-core-soc-solutions/02-introduction-to-siem.md) — Collection, parsing, normalisation, search, correlation and retention limitations ko understand karna.
- [Splunk: The Basics](03-core-soc-solutions/03-splunk-the-basics.md) — Search pipeline ko incremental filters, field checks and aggregations ke saath build karna.
- [Elastic Stack: The Basics](03-core-soc-solutions/04-elastic-stack-the-basics.md) — Elastic/Kibana mein time range, data view, field existence aur event context validate karna.
- [Introduction to SOAR](03-core-soc-solutions/05-introduction-to-soar.md) — Playbooks, connectors, enrichment and controlled response automation ko evaluate karna.

### 04 · Cyber Defence Frameworks

Observed behaviour ko frameworks se explain karna, bina framework label ko evidence ka replacement banaye.

- [Pyramid of Pain](04-cyber-defence-frameworks/01-pyramid-of-pain.md) — Indicators ko attacker ke liye change karne ki cost aur defender ke detection value se compare karna.
- [Cyber Kill Chain](04-cyber-defence-frameworks/02-cyber-kill-chain.md) — Observed activity ko high-level attack lifecycle ke stages ke saath map karna.
- [Unified Kill Chain](04-cyber-defence-frameworks/03-unified-kill-chain.md) — Multiple attack phases ko broader adversary objectives and defensive opportunities se interpret karna.
- [MITRE ATT&CK](04-cyber-defence-frameworks/04-mitre-attandck.md) — Evidence ko ATT&CK tactics/techniques se map karna and defensive coverage gaps ko explain karna.
- [Summit](04-cyber-defence-frameworks/05-summit.md) — Scenario evidence ko structured hypothesis, timeline aur defensive opportunities mein convert karna.
- [Eviction](04-cyber-defence-frameworks/06-eviction.md) — Eradication/recovery decisions ko evidence, scope and validation se justify karna.

### 05 · Phishing Analysis

Email context, headers, URL/attachment clues aur human reporting ko combine karke a defensible assessment banana.

- [Phishing Analysis Fundamentals](05-phishing-analysis/01-phishing-analysis-fundamentals.md) — Email identity, routing headers, authentication results and message context ko correlate karna.
- [Phishing Emails in Action](05-phishing-analysis/02-phishing-emails-in-action.md) — Multiple email artefacts aur business context se a defensible benign/suspicious assessment build karna.
- [Phishing Analysis Tools](05-phishing-analysis/03-phishing-analysis-tools.md) — Analysis utilities ke output ko source, freshness and limitations ke saath interpret karna.
- [Phishing Prevention](05-phishing-analysis/04-phishing-prevention.md) — Prevention ko identity controls, mail controls, user reporting and response readiness ke saath map karna.
- [The Greenholt Phish](05-phishing-analysis/05-the-greenholt-phish.md) — A case-style phishing assessment ko fact/hypothesis separation aur an evidence-led summary ke saath likhna.
- [Snapped Phishing Line](05-phishing-analysis/06-snapped-phishing-line.md) — User-reported suspicious communication ko triage, scope and hand off karna.
- [Phishing Unfolding](05-phishing-analysis/07-phishing-unfolding.md) — A developing phishing scenario ko evolving evidence, scope changes and status updates ke through analyse karna.

### 06 · Network Traffic Analysis

Packet metadata, conversations, protocols aur capture limitations se network hypotheses validate karna.

- [Network Traffic Basics](06-network-traffic-analysis/01-network-traffic-basics.md) — Flows, endpoints, ports, protocols, direction and time window ko accurately describe karna.
- [Wireshark: The Basics](06-network-traffic-analysis/02-wireshark-the-basics.md) — Display filters, packet details, conversations and stream context ko samajhna.
- [Wireshark: Packet Operations](06-network-traffic-analysis/03-wireshark-packet-operations.md) — Packet navigation, marking, stream reconstruction and export functions ka careful use.
- [Wireshark: Traffic Analysis](06-network-traffic-analysis/04-wireshark-traffic-analysis.md) — PCAP ko hypothesis-led analysis aur corroborated protocol evidence se explore karna.
- [NetworkMiner](06-network-traffic-analysis/05-networkminer.md) — Extracted network artefacts ko source-PCAP context and provenance ke saath interpret karna.

### 07 · Network Security Monitoring

Network discovery, interception aur unusual transfer patterns ko baseline aur context ke saath assess karna.

- [Network Security Essentials](07-network-security-monitoring/01-network-security-essentials.md) — Network controls, segmentation, firewall telemetry and normal communication patterns ka relationship samajhna.
- [Network Discovery Detection](07-network-security-monitoring/02-network-discovery-detection.md) — Scanning/discovery hypotheses ko rate, target diversity, response patterns and baseline se assess karna.
- [Data Exfiltration Detection](07-network-security-monitoring/03-data-exfiltration-detection.md) — Unusual egress ko bytes, destinations, identity, timing and business context ke saath evaluate karna.
- [Man-in-the-Middle Detection](07-network-security-monitoring/04-man-in-the-middle-detection.md) — TLS/certificate anomalies, ARP/DHCP changes and path context ko investigate karna.
- [IDS Fundamentals](07-network-security-monitoring/05-ids-fundamentals.md) — Signature/anomaly alerts, rule logic, false positives and sensor placement ko samajhna.
- [Snort](07-network-security-monitoring/06-snort.md) — Snort alert/rule semantics and packet-level evidence ko interpret karna.

### 08 · Web Security Monitoring

HTTP request/response patterns aur server telemetry se suspicious web activity identify karna.

- [Web Security Essentials](08-web-security-monitoring/01-web-security-essentials.md) — HTTP method, path, status, headers, client identity and server-side context ko combine karna.
- [Detecting Web Attacks](08-web-security-monitoring/02-detecting-web-attacks.md) — Abnormal request sequences, input patterns and response behaviour ko defensive telemetry mein recognise karna.
- [Detecting Web Shells](08-web-security-monitoring/03-detecting-web-shells.md) — Unexpected server-side files, process behaviour and web-request patterns ko correlate karna.
- [Detecting Web DDoS](08-web-security-monitoring/04-detecting-web-ddos.md) — Request rate, source distribution, endpoint concentration and service health ko baseline ke against analyse karna.

### 09 · Windows Security Monitoring

Windows event channels, logon/process context aur timeline correlation ko defensive investigation mein use karna.

- [Windows Logging for SOC](09-windows-security-monitoring/01-windows-logging-for-soc.md) — Windows event channels, providers, logon/process context and event-time semantics ka foundation banana.
- [Windows Threat Detection 1](09-windows-security-monitoring/02-windows-threat-detection-1.md) — Windows telemetry se clear hypothesis banane aur process/account context check karne ki practice.
- [Windows Threat Detection 2](09-windows-security-monitoring/03-windows-threat-detection-2.md) — Related Windows events ko timeline, parent-child relationship and identity changes ke across correlate karna.
- [Windows Threat Detection 3](09-windows-security-monitoring/04-windows-threat-detection-3.md) — Multiple telemetry sources ko combine karke bounded, reproducible host investigation banana.

### 10 · Linux Security Monitoring

Linux authentication, service, process aur system logs ko correlate karna.

- [Linux Logging for SOC](10-linux-security-monitoring/01-linux-logging-for-soc.md) — Authentication, systemd journal, sudo and service logs ki source and reliability samajhna.
- [Linux Threat Detection 1](10-linux-security-monitoring/02-linux-threat-detection-1.md) — Linux authentication and system events se suspicious patterns ko normal administration se separate karna.
- [Linux Threat Detection 2](10-linux-security-monitoring/03-linux-threat-detection-2.md) — Process/service changes aur authentication events ke beech relation ko investigate karna.
- [Linux Threat Detection 3](10-linux-security-monitoring/04-linux-threat-detection-3.md) — Host investigation findings ko timeline, impact and remaining telemetry gaps ke saath report karna.

### 11 · Malware Concepts for SOC

Malware behaviour, safe file triage aur legitimate tools ke suspicious use ko samajhna.

- [Malware Classification](11-malware-concepts-for-soc/01-malware-classification.md) — Malware families/categories ko capability and behaviour ke basis par compare karna.
- [Intro to Malware Analysis](11-malware-concepts-for-soc/02-intro-to-malware-analysis.md) — Static and controlled dynamic analysis ke goals, safety boundaries and evidence preservation samajhna.
- [Living Off the Land Attacks](11-malware-concepts-for-soc/03-living-off-the-land-attacks.md) — Legitimate signed tools ko suspicious context, ancestry and command behaviour ke saath assess karna.
- [Shadow Trace](11-malware-concepts-for-soc/04-shadow-trace.md) — A named scenario ko independent forensic observations, timelines and confidence statements ke through analyse karna.

### 12 · Threat Analysis Tools

Threat-intelligence indicators ko source, time, context aur confidence ke saath assess karna.

- [Intro to Cyber Threat Intel](12-threat-analysis-tools/01-intro-to-cyber-threat-intel.md) — Intelligence lifecycle, collection context, source reliability and confidence ka practical use.
- [File and Hash Threat Intel](12-threat-analysis-tools/02-file-and-hash-threat-intel.md) — File hashes, metadata and multi-source reputation ko reproducible triage mein use karna.
- [IP and Domain Threat Intel](12-threat-analysis-tools/03-ip-and-domain-threat-intel.md) — IP/domain enrichment ko DNS, hosting, certificate, time and local telemetry ke saath interpret karna.
- [Invite Only](12-threat-analysis-tools/04-invite-only.md) — Restricted or invite-only themed content ke around safe intelligence-handling and evidence documentation practices apply karna.

### 13 · SIEM Triage for SOC

SIEM observations ko investigation scope, correlation, verdict aur handoff mein convert karna.

- [Log Analysis with SIEM](13-siem-triage-for-soc/01-log-analysis-with-siem.md) — Multiple log sources ko normalise, filter and correlate karke a reproducible story banana.
- [Alert Triage With Splunk](13-siem-triage-for-soc/02-alert-triage-with-splunk.md) — Splunk observations ko triage verdict, supporting evidence and next action mein translate karna.
- [Alert Triage With Elastic](13-siem-triage-for-soc/03-alert-triage-with-elastic.md) — Elastic search results ko fields, mappings, data view and correlated evidence ke saath assess karna.
- [ItsyBitsy](13-siem-triage-for-soc/04-itsybitsy.md) — Small-scope scenario triage ko structured evidence, timeline and proportionate confidence ke saath practise karna.
- [Benign](13-siem-triage-for-soc/05-benign.md) — Benign-looking or false-positive candidates ko evidence aur baseline se validate karna.

### 14 · SOC Level 1 Capstone Challenges

Multiple artefacts ko combine karke a coherent incident timeline, impact assessment aur response report banana.

- [Tempest](14-soc-level-1-capstone-challenges/01-tempest.md) — End-to-end incident storyline ko evidence-linked timeline, scope and response recommendations mein convert karna.
- [Boogeyman 1](14-soc-level-1-capstone-challenges/02-boogeyman-1.md) — Scenario artefacts ko isolated clues ke bajay linked evidence set ke roop mein dekhna.
- [Boogeyman 2](14-soc-level-1-capstone-challenges/03-boogeyman-2.md) — Case scope ko new evidence ke basis par update karna and cross-host/identity links verify karna.
- [Boogeyman 3](14-soc-level-1-capstone-challenges/04-boogeyman-3.md) — Multi-stage case ko coherent report with confidence, impact and response decision points mein close karna.
- [Hidden Hooks](14-soc-level-1-capstone-challenges/05-hidden-hooks.md) — Unexpected clues ko evidence-led pivots mein transform karna, without assuming every anomaly is meaningful.
- [Open Door](14-soc-level-1-capstone-challenges/06-open-door.md) — A case narrative ko authorised scope, clear evidence and defensible response recommendations ke saath structure karna.

## Reporting templates

Use the shared [post-lab write-up template](../writeup-template.md), [alert triage notes](../01-alert-triage.md), [phishing notes](../02-phishing-analysis.md), [SIEM notebook](../03-siem-triage-notebook.md), [network analysis notes](../04-network-traffic-analysis.md), [host telemetry notes](../05-host-telemetry.md), [threat intelligence and malware notes](../06-threat-intelligence-and-malware.md), and [capstone report guide](../07-capstone-report-guide.md).

## Quality checklist

- [ ] Scope, timezone and evidence source are explicit.
- [ ] Facts are separate from hypotheses and conclusions.
- [ ] Query/filter and data limitations are recorded.
- [ ] At least one benign alternative is considered.
- [ ] Confidence and next action are justified.
- [ ] No flags, active-room answers, restricted screenshots or step-by-step active-room solutions are included.

**Room inventory basis:** room names are mapped from the current public path listing and this repository's existing room map. TryHackMe may change path content; use the official path as the source of truth.
