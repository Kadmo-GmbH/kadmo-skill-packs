---
name: Dashboard Overview
domain: schufa.de
path: /
parent: schufa.de
tags: ["@dashboard", "@navigation", "@landing"]
learned: "2026-05-11"
last_verified: "2026-05-11"
confidence: 0.30
---

# Dashboard Overview

The Start page is the authenticated landing surface of `app.schufa.de`. It greets the user by first name and surfaces two primary CTAs — one routes to the score breakdown, the other to the stored data entries — plus a teaser card for the Identitätsschutz product.

## Page Structure

```
+-- Banner (logo, profile avatar chip) ---------------+
+-- Tabs: Start | Score & Bonität | Identitätsschutz | Service +
|                                                      |
|  "Hallo {Vorname},"                                   |
|  [Zur Score-Berechnung] -> /bonidata/ngs              |
|                                                      |
|  "Welche Daten speichert die SCHUFA über mich?"      |
|  - Anfragen                                          |
|  - Vertragsdaten                                     |
|  - Zahlungsstörungen                                 |
|  [Zu den Einträgen] -> /bonidata                     |
|                                                      |
|  "Mit der SCHUFA sicher im Netz surfen?"             |
|  [Zum Identitätsschutz] -> /identitaetsschutz        |
|                                                      |
|  [Hilfe benötigt?]   [Feedback geben]                |
+--+ Footer (Datenschutz | AGB | Impressum | Cookies) +
```

## Key Elements

| Element | Selector hint | Notes |
|---|---|---|
| Greeting heading | `main h1`, text starts with "Hallo " | Includes user's Vorname |
| Score CTA | `button:has-text("Zur Score-Berechnung")` | Navigates to `/bonidata/ngs` |
| Entries CTA | `button:has-text("Zu den Einträgen")` | Navigates to `/bonidata` |
| Identitätsschutz CTA | `button:has-text("Zum Identitätsschutz")` | Navigates to `/identitaetsschutz` |
| Help button | `button:has-heading("Hilfe benötigt?")` | Opens help modal or routes to Service |
| Feedback button | `button:has-heading("Feedback geben")` | Opens product feedback survey |

## Common Tasks

**Verify the user is logged in.** Navigate `/`, snapshot, assert presence of the "Hallo {firstname}," heading. If a login form appears instead, session expired.

**Jump to score detail.** Click "Zur Score-Berechnung" — destination is `/bonidata/ngs`.

**Jump to data entries.** Click "Zu den Einträgen" — destination is `/bonidata`.

## Coverage Status

Confidence is low because only the surface layout has been observed; the help-modal contents, feedback-survey flow, and per-element a11y attributes have not been verified.
