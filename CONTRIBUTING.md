# Contributing to SOC-Notes

Thanks for contributing notes, screenshots, class material, commands or investigation write-ups. The goal is a field manual another analyst can follow—not a raw notes dump.

## Note quality bar

Each topic should answer as many of these as relevant:

1. **What is it?** Define the concept and its boundaries.
2. **Why does it matter to a SOC?** Explain the analyst decision it informs.
3. **What evidence is required?** Name the source, fields and known collection gaps.
4. **How is it investigated?** Give ordered, reproducible steps.
5. **Can the example be tested?** Include synthetic positive and benign/negative cases.
6. **What can mislead the analyst?** Document false positives, assumptions and limits.
7. **Where can it be verified?** Link official or primary references.

## Suggested writing format

- Clear title and a 2–3 sentence purpose statement
- Small summary table for quick scanning
- Diagram where it genuinely clarifies architecture or flow
- Copyable query/rule with an explanation of key fields
- Triage checklist and at least one benign alternative
- Version / platform caveats where syntax depends on tooling
- Sources and last-reviewed date when freshness matters

## Evidence and privacy rules

- Do not commit passwords, API keys, tokens, private keys, session cookies or personal data.
- Sanitize internal IPs/domains, hostnames, usernames, case IDs and customer details before public release.
- Use synthetic examples or reserved example domains when the real values are not necessary.
- Never upload customer logs or incident evidence without explicit authorization and appropriate handling approval.
- Do not present a lab hypothesis as a confirmed incident, or an untested detection as production-ready.

## Status labels

Use explicit wording where useful:

- **Illustrative:** demonstrates an idea; not tested against a live platform.
- **Lab-tested:** tested in the stated environment/version; describe how.
- **Production pattern:** only when operational context and validation are documented; this label does not replace local review.

## Review checklist

- [ ] Relative links resolve.
- [ ] Queries/rules identify assumed fields and data sources.
- [ ] Positive, benign and edge-case behavior is discussed.
- [ ] Time zone and scope are stated for incident examples.
- [ ] Syntax is tested or clearly labeled illustrative.
- [ ] No secrets, personal data or unauthorized evidence.
- [ ] References support the relevant claims.
