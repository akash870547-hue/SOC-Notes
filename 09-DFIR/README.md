# 09 · Digital Forensics & Incident Response (DFIR)

## Objective

Preserve, examine and explain digital evidence in a repeatable and defensible way. Work within authorization and use a procedure appropriate to the system and incident.

~~~mermaid
flowchart LR
 A[Authorize + scope] --> B[Identify sources]
 B --> C[Collect / acquire]
 C --> D[Hash + record]
 D --> E[Analyze working copy]
 E --> F[Timeline + correlate]
 F --> G[Findings + limitations]
 G --> H[Review / handoff]
~~~

## Evidence log

Record a unique ID, description, device/path, collector and authority, collection time/timezone, method/tool version, original and working-copy locations, SHA-256 at acquisition/verification, storage/access history, and limitations.

## Hashing examples

PowerShell:
~~~powershell
Get-FileHash -Algorithm SHA256 -LiteralPath .\evidence.evtx
~~~

Linux:
~~~bash
sha256sum evidence.evtx
~~~

A matching hash supports that bytes are unchanged between checks; it does not establish origin or completeness.

## Timeline normalization

Keep the original timestamp and timezone. Add normalized UTC for correlation without overwriting the original. Note suspected clock drift, daylight-saving ambiguity and ingestion delay.

| UTC time | Original time | Source | Entity | Evidence ID | Interpretation | Confidence |
|---|---|---|---|---|---|---|

## Report questions

1. What happened, and when can it be established?
2. Which systems, identities and data are affected or potentially affected?
3. Which evidence directly supports each finding?
4. What alternative explanations were tested?
5. What actions were taken, by whom, and with what result?
6. What evidence or telemetry was unavailable?
7. What remains uncertain, and what would resolve it?

Do not casually modify original evidence. Avoid running unknown files on production endpoints. Use vetted, isolated analysis environments and retain commands/tool versions for reproducibility.
