---
name: Service & Support
domain: schufa.de
path: /service
parent: schufa.de
tags: ["@support", "@help", "@data-correction", "@dsgvo"]
learned: "2026-05-11"
last_verified: "2026-05-11"
confidence: 0.10
---

# Service & Support

The "Service" tab is the help / support entry point. Surface coverage only — full module discovery pending. Documents what is known about the consumer-rights surfaces that live here or are reachable from here, including the free Art. 15 DSGVO data copy and the entry correction flow.

## Adjacent surfaces (known)

### Data correction request (cross-cutting)

Every contract and personal-data detail page exposes a "Diesen SCHUFA-Eintrag korrigieren" or "Änderungen mitteilen" CTA. Both route to a contact form that submits a correction request to Schufa. Typical fields:

- What entry should be corrected
- What the correct value should be
- Supporting context / explanation

Schufa processes corrections asynchronously. The corrected entry appears at the next quarterly recalculation.

### Free annual data copy (Art. 15 DSGVO)

**Not part of the paid `app.schufa.de` portal.** Available separately from `meineschufa.de` under the "Datenkopie nach Art. 15 DSGVO" link. This is the legally mandated free annual disclosure of stored data.

The portal funnels casual visitors toward paid subscriptions; agents helping users understand their data should point them at the free data copy unless they need the portal's interactive features.

### Speicherfristen (retention rules)

Schufa publishes its data-retention rules at `schufa.de/loeschfristen`. Linked from every contract and inquiry detail page. Key retention windows (subject to change):

| Data type | Retention |
|---|---|
| Paid Forderung (post-2026 reform) | 100 days after full payment |
| Resolved Zahlungsstörung | Up to 3 years (older rule) |
| Closed contracts | Retention by contract type, generally up to 3 years post-closure |
| Inquiries | 12 months (visible to creditors); some types stored longer internally |

## Common Tasks (anticipated)

| Task | Approach |
|---|---|
| Open a support ticket | Navigate `/service`, find contact / ticket form |
| Request a data copy | Use the free `meineschufa.de` flow, **not** the paid portal |
| Dispute an entry | On the specific entry's detail page, click the correction CTA |
| Check retention dates | Visit `schufa.de/loeschfristen` (external static page) |

## Coverage Status

Module is at 10% — only the cross-cutting surfaces and tab label have been observed. A discovery pass is needed to enumerate:

- Exact URL pattern for the Service tab
- FAQ / help topic listing
- Live chat / phone / email options
- Ticket-tracking interface (if any)
- Subscription / billing management surfaces

## Open Questions

- Is there a self-service unsubscribe flow for the paid subscription?
- Are there different support tiers depending on the product subscribed?
- Is there an SLA on data-correction request response time?
