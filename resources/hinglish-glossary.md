# SOC Glossary — English + Hinglish

A quick bilingual reference. Definitions are intentionally short; always use the organization's official terminology when it differs.

| Term | Plain-English meaning | Hinglish explanation |
|---|---|---|
| Alert | A rule/analytic has flagged activity for review | Tool ne kisi behaviour ko review ke liye flag kiya; attack confirm nahi hua |
| Event / log | A recorded observation | System ki ek recorded activity; akela event normal bhi ho sakta hai |
| Triage | Initial validation and prioritization | Pehle alert ki details check karke decide karna ki kitni urgency aur next step kya hai |
| Correlation | Linking related events across sources/time | Alag logs ko time, account, host ya IP ke basis par jodkar pattern samajhna |
| Telemetry | Data collected from endpoints, network, identity, cloud etc. | System se milne wala evidence-data; collection miss hua to investigation weak hogi |
| Baseline | What is normal for a particular environment | Apne environment ka normal behaviour; har company ka baseline alag ho sakta hai |
| IOC | Observable potentially associated with compromise | Possible compromise ka clue, jaise hash/domain/IP; match alone proof nahi |
| IOA | Behaviour suggesting an attack may be underway | Attacker kya kar raha ho sakta hai, us behaviour ka clue |
| TTP | Tactics, techniques and procedures | Attacker ka objective, tareeqa aur actual procedure |
| False positive | Alert fired, but intended suspicious behaviour/risk was not present | Alert aaya par jis risk ke liye rule bana tha woh evidence se support nahi hua |
| False negative | Relevant behaviour occurred but detection did not identify it | Activity hui par detection ne pakda nahi; data gap bhi cause ho sakta hai |
| Severity | Potential impact / urgency | Agar situation real hai to nuksaan kitna serious ho sakta hai |
| Confidence | Strength of supporting evidence | Available evidence conclusion ko kitna strongly support karta hai |
| Escalation | Passing a case to the proper response tier/owner | Case ko sahi L2/IR/team tak evidence ke saath pahunchana |
| Containment | Limiting an incident's spread/impact | Incident ko aur failne se rokna; approved playbook aur authorization follow karo |
| Eradication | Removing the verified cause/artifacts | Confirmed malicious cause ko hatana, policy aur evidence handling ke mutabiq |
| Recovery | Returning systems/services to trusted operation | Systems ko safe state se service mein wapas lana aur recurrence monitor karna |
| Persistence | Maintaining access across logoff/restart/change | Restart ya password change ke baad bhi access banaye rakhne ka tareeqa |
| Lateral movement | Moving between systems/accounts inside an environment | Ek host/account se doosre tak move karna |
| Exfiltration | Unauthorized transfer of data out of an environment | Sensitive data ko environment se bahar le jana |
| C2 / command and control | Communication used to coordinate compromised systems | Compromised system aur controller ke beech communication; normal traffic jaisa bhi dikh sakta hai |
| SIEM | Platform to collect, search and correlate security logs | Central place jahan logs aate hain aur analyst searches/rules se signals nikalta hai |
| EDR / XDR | Endpoint / cross-domain detection and response | Endpoint ya multiple sources ki activity dekhne, investigate karne aur approved response mein madad dene wale tools |
| SOAR | Security workflow automation/orchestration | Repetitive response steps ko playbooks se automate karna; bad playbook ko automation aur tez kar sakti hai |
| Chain of custody | Record of who handled evidence and when | Evidence kisne collect, transfer, store ya access kiya—uska traceable record |
| Hash | Fixed-length digest calculated from file/data | File ka fingerprint; matching hash integrity check mein helpful hai, par maliciousness prove nahi karta |
| False closure | Closing without enough evidence or rationale | Bina proper validation ke ticket band kar dena; uncertainty aur gaps likhne chahiye |
| Detection tuning | Improving rules to reduce noise and preserve useful signals | Rule ko test karke better banana, false positives kam karna aur meaningful behaviour miss na karna |
| Threat hunting | Hypothesis-led search for suspicious behaviour | Ek testable hypothesis bana kar logs mein proactively evidence dhoondhna |
| Telemetry gap | Missing/insufficient visibility | Required logs ya fields available nahi; absence ko “no attack” nahi bol sakte |

## Quick analyst language

- **Observed:** “We saw X in source Y at time Z.” / “Source Y mein time Z par X record hua.”
- **Assessed:** “This is consistent with X, but evidence is incomplete.” / “Pattern X se match karta hai, par evidence abhi incomplete hai.”
- **Confirmed:** “Independent evidence supports the conclusion.” / “Alag evidence sources conclusion ko support karte hain.”
- **Unknown:** “The available telemetry cannot establish X.” / “Maujood logs se X prove ya rule out nahi kiya ja sakta.”
