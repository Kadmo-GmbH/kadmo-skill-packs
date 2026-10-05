---
name: Data Entries Overview
domain: schufa.de
path: /bonidata
parent: schufa.de
tags: ["@contracts", "@inquiries", "@personal-data", "@vertragsdaten", "@anfragen"]
learned: "2026-05-11"
last_verified: "2026-05-11"
confidence: 0.70
---

# Data Entries Overview

The `/bonidata` page is the consolidated view of what Schufa stores about the user. It pairs the score summary with four data categories: **Anfragen** (inquiries), **Vertragsdaten** (contracts), **Zahlungsstörungen** (payment defaults), and **Personendaten** (personal data + reported addresses). Each category is a click-through to a detail list.

## Page Structure

```
+-- Mein SCHUFA-Score card ----------+ +-- Einträge card -----------+
|  Ermittelt am: DD.MM.YYYY          | |  Ermittelt am: DD.MM.YYYY  |
|  Aktualisierung: alle 3 Monate     | |  Aktualisierung: alle 3 Mo |
|  [Score badge]                     | |  Anfragen        N  ▶      |
|  Band label                        | |  Vertragsdaten   N  ▶      |
|  [Zur Score-Berechnung]            | |  Zahlungsstörungen N ▶     |
+-----------------------------------+ +----------------------------+
+-- Gemeldete Personendaten card --+ +-- Der neue SCHUFA-Score --+
|  Personendaten   N  ▶            | |  [Score-Tutorial öffnen]   |
+----------------------------------+ +----------------------------+
+-- Feedback card -----------------+
|  [Zur Umfrage]                   |
+----------------------------------+
```

## Calculation cadence

Both cards show "Aktualisierung: alle 3 Monate" — entries and score recalculate once per quarter. The "Ermittelt am" date is the cutoff for the current snapshot.

## Sub-pages

### Anfragen — `/bonidata/anfragen` (path tentative)

List of every Bonitätsprüfung filed in the **last 12 months**, ordered most recent first. Each entry shows date + inquiry type + requesting company.

**Inquiry types observed:**

| Type | Counts toward score? | Notes |
|---|---|---|
| Bonitätsprüfung Kreditkonditionen | ❌ No | Konditionsanfrage — rate shopping, score-neutral |
| Bonitätsprüfung | ⚠️ Maybe | Bare credit check; may count under factor #2 or #3 |
| Bonitätsanfrage / Kreditanfrage | ✅ Yes | Hard inquiry tied to application — hits factor #2 or #3 |
| Identitäts-/Altersprüfung | ❌ No | Identity/age verification only |

**Common requestors:** Consumer banks running rate-shopping inquiries, mobility platforms, online retailers running BNPL credit checks, telecom providers, insurance providers.

**Per-inquiry detail page** shows the full company name, the legal basis, and the data shared.

### Vertragsdaten — `/bonidata/vertragsdaten`

List of every contract Schufa has on file, ordered most recent first. Each entry shows the report date, contract type, and provider.

**Contract types observed:**

| Type | Icon | Schufa factor relevance |
|---|---|---|
| Girokonto | bank | Factor #7 (Bankvertrag age) |
| Kreditkarte | bank | Factors #7 + #10 |
| Ratenkredit | hand-coin | Factors #4, #11, #12 |
| Kredit (grundpfandrechtlich gesichert) | hand-coin | Factor #5 (mortgage); excluded from factor #4 |
| Leasingvertrag | bank | Factor #7 |
| Bürgschaft | bank | Factor #7 |
| Bauspardarlehen | bank | Factor #7 (may also report as Ratenkredit) |

**Per-contract detail page** shows:

- Contract type + provider (legal name)
- "Übermittelt am" (date reported to Schufa)
- For Ratenkredit: **Fälligkeit 1. Rate**, **Kontonummer**, **Kreditbetrag**, **Anzahl Raten**, **Zahlweise**
- "Enthalten in der Score-Berechnung vom DD.MM.YYYY" link with chips for the factors this contract feeds (e.g., "Längste Restlaufzeit aller Ratenkredite", "Kreditstatus")
- "Speicherfrist" explanation card with link to `schufa.de/loeschfristen`
- "Diesen SCHUFA-Eintrag korrigieren" button → opens a contact form to dispute the entry

### Zahlungsstörungen — `/bonidata/zahlungsstoerungen` (path tentative)

List of registered payment defaults. **Empty state** displays: *"Es liegen keine Zahlungsstörungen vor – alles in Ordnung!"*

A non-empty list is catastrophic for factor #1 (Keine Zahlungsstörung, max 264 P). Each entry would show the date, creditor, amount, and resolution status.

**Empty state is the goal** — agents auditing a profile should always confirm this list shows 0 entries.

### Personendaten — `/bonidata/personendaten` (path tentative)

Two-section page:

**Persönliche Informationen**

| Field | Notes |
|---|---|
| Vorname / Nachname | First / last name as reported |
| Geburtsdatum | Date of birth |
| SCHUFA-Datensatznummer | Unique Schufa record ID (alphanumeric) |
| "Änderungen mitteilen" button | Opens data-correction form |

**Gemeldete Anschriften**

Ordered list of every address Schufa has on file, current address first. Each entry shows the report date and is clickable to view the full street/city.

The **current address date** is the input to factor #9. Agents should expect the displayed date to be the start of the most recent residency, **not** the date the address was reported (these may differ by weeks if reporting was delayed).

## Common Tasks

| Task | Approach |
|---|---|
| Audit for stale / wrong entries | Navigate `/bonidata` → click each of the 4 categories → cross-reference against the user's records |
| Count score-affecting inquiries | Open Anfragen, filter out "Bonitätsprüfung Kreditkonditionen" and "Identitäts-/Altersprüfung" — remainder counts toward factors #2 / #3 |
| Verify the oldest bank contract | Open Vertragsdaten → scroll to last entry → confirm date and provider |
| Verify mortgage on file | Open Vertragsdaten → filter for "Kredit (grundpfandrechtlich gesichert)" — at least one entry = factor #5 maxed |
| Verify identity status | Open Personendaten → if Datensatznummer is present and address history is populated, factor #6 is typically maxed; cross-check by reading factor #6 detail on `/bonidata/ngs` |
| File a correction | On any detail page, click "Jetzt Kontakt aufnehmen" / "Änderungen mitteilen" |

## Gotchas

- **Mortgages may also show up as Ratenkredit** when the provider reports both products. The Vertragsdaten list is sourced from creditor reports, not from Schufa categorization — so the same loan may appear once as "Ratenkredit" and once as "Kredit (grundpfandrechtlich gesichert)". Schufa de-duplicates these for scoring but the UI shows both.
- **Closed contracts can still appear** within Schufa's retention period (Speicherfrist). The portal does not always make it clear that a contract is closed — agents should treat any entry as "potentially active" unless cross-verified.
- **The "Anzahl Raten" field on a Ratenkredit is total rates, not remaining rates.** To estimate the remaining term, subtract elapsed months from this number and check against factor #11's tier table.
- **Address history can include very short stays** (e.g., 3-month interim addresses). These count toward residency history but reset the factor #9 timer each time the "current" address changes.
- **Personal data corrections do not change the score immediately.** A successful correction is reflected at the next quarterly recalculation.
