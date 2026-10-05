---
name: Identity Protection
domain: schufa.de
path: /identitaetsschutz
parent: schufa.de
tags: ["@identity", "@privacy", "@data-leak", "@monitoring"]
learned: "2026-05-11"
last_verified: "2026-05-11"
confidence: 0.10
---

# Identity Protection

The "Identitätsschutz" tab markets a monitoring service that scans the open web and known dark-web data-leak collections for the user's stored personal data over the past 90 days. Surface coverage only — full module discovery pending.

## Page Structure (surface)

Teaser card observed on the Start page:

```
+-- "Mit der SCHUFA sicher im Netz surfen?" ----------+
|  "Unser Service steht Ihnen jetzt zur Verfügung!     |
|   Im Identitätsschutz können Sie Ihre Daten          |
|   hinterlegen und manuell prüfen, ob diese in den    |
|   vergangenen 90 Tagen im Internet geleakt wurden."  |
|  [Zum Identitätsschutz]                              |
+------------------------------------------------------+
```

The full Identitätsschutz dashboard has not been explored. Expected sub-features based on the teaser copy:

- Data submission form (email, phone, DOB, address) — what to monitor
- Manual scan trigger
- Scan results / history list
- Per-leak detail (source, exposed fields, mitigation suggestions)
- Notification preferences

## Common Tasks (anticipated)

| Task | Approach |
|---|---|
| Run a leak scan | Navigate `/identitaetsschutz`, submit monitored identifiers, trigger scan |
| Review prior scan results | Navigate `/identitaetsschutz`, list page should show scan history |
| Update monitored identifiers | Navigate `/identitaetsschutz` → settings sub-page |

## Coverage Status

Module is at 10% — only the entry-point teaser has been observed. A discovery pass is needed to enumerate:

- Exact URL pattern
- Submitted-data form fields and validation
- Scan trigger flow and async result delivery
- Leak record schema (source, exposed fields, date)
- Notification mechanisms
- Any tie-in with the rest of the Schufa data model (does a leak hit affect the score? expected answer: no, since the 12 factors do not include identity-leak signals)

## Open Questions

- Is the service included in the Schufa subscription or an upsell?
- How are matches scored / weighted? (e.g., partial matches vs full credential pairs)
- Does the service include credit-monitoring alerts when new entries appear in the Schufa file?
