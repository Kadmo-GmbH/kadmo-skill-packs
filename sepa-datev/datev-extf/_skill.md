---
name: EXTF Format & SKR Charts
path: /datev-extf
tags: ["@qa", "@billing", "@datev", "@german"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.75
needs_reverification: false
notes: "Verified against the official DATEV sample file EXTF_Buchungsstapel.csv (Muster bundle, file dated 2025-06-18) and DATEV help documents; known discrepancies between DATEV's published field regexes and DATEV's own samples are recorded as a gotcha. Extracted into this community pack 2026-08-12."
---

# DATEV Export — EXTF Format & SKR Charts

## Overview

The accounting-export half of German billing QA: the EXTF/Buchungsstapel CSV that third-party
systems hand to a tax adviser's DATEV, its header and record format, the traps that import cleanly
and book wrongly, and the SKR chart-of-accounts landscape — including the non-profit tax spheres
whose loss is this module's highest-value test.

The payment-rails half is [sepa-direct-debit](../sepa-direct-debit/_skill.md); the shared failure
taxonomy, fixture matrix and validation heuristics live in this pack's [root](../_skill.md).

## The EXTF/Buchungsstapel Format

Verified against the **official DATEV sample file** `EXTF_Buchungsstapel.csv` from DATEV's public
Muster bundle (file dated 2025-06-18). Structure:

- **Line 1** — the header, **31 fields**
- **Line 2** — column names, **125 columns**
- **Line 3 onward** — booking records

Header fields, by position (values shown are the sample's):

| # | Field | Sample | Meaning |
|---|-------|--------|---------|
| 1 | Kennzeichen | `"EXTF"` | `EXTF` = written by a third-party program. `DTVF` is reserved for DATEV's own tools — **a club system must emit `EXTF`** |
| 2 | Versionsnummer | `700` | Header version. **`700` is the only current value**; `300`/`510` are legacy (DATEV Rechnungswesen V3.0 / V5.1) |
| 3 | Datenkategorie | `21` | **`21` = Buchungsstapel.** `16` = Debitoren/Kreditoren, `20` = Sachkontenbeschriftungen, `65` = wiederkehrende Buchungen |
| 4 | Formatname | `"Buchungsstapel"` | Must agree with field 3 |
| 5 | Formatversion | `13` | Current. v13 has 125 record fields, v12 had 124. Earlier versions are shorter — DATEV's 2020 field description (Dok. 1003221) enumerates only 120 — so do not assume a fixed count below v12 |
| 6 | erzeugt am | `20240130140440439` | 17 digits — `YYYYMMDDHHMMSSFFF` |
| 11 | Berater | `29098` | Tax adviser number, 4–7 digits |
| 12 | Mandant | `55003` | Client number, 1–5 digits |
| 13 | WJ-Beginn | `20240101` | Fiscal-year start, `YYYYMMDD`. **Supplies the year for `Belegdatum`** |
| 14 | Sachkontenlänge | `4` | Single digit 4–8. **Must match the target client's DATEV configuration** |
| 15/16 | Datum von / bis | `20240101` / `20240831` | Period covered |
| 19 | Buchungstyp | `1` | `1` = Finanzbuchführung, `2` = Jahresabschluss |
| 21 | Festschreibung | `0` | `0` = not locked, `1` = locked. The official sample emits `0`. The *provable* auto-lock default lives on **record** field 114, not here — see the traps table |
| 22 | WKZ | `"EUR"` | Currency |
| 27 | Sachkontenrahmen | `"03"` | Which SKR the batch is booked against — see below |

**File-level rules that cause silent non-import** (DATEV help document 1044208): the filename must
begin `EXTF_` and end `.csv`; a Buchungsstapel holds at most **99,999 rows** (a 5-digit record counter) — once Saldo- and Gruppensummenzeilen are counted the usable posting count is **99,997**, so the boundary trips before 100,000; the client's
Berater/Mandant/fiscal-year must match the target ledger; and if line 1 is missing, DATEV simply
does not offer the file for import. DATEV's own diagnostic is to open the file and check that cell
A1 reads `EXTF` — if it reads `Umsatz (ohne Soll/Haben-Kz)`, the header line was dropped.

Record field order — the first 14 columns, which carry the booking:

```
1 Umsatz (ohne Soll/Haben-Kz) │ 2 Soll/Haben-Kennzeichen │ 3 WKZ Umsatz │ 4 Kurs
5 Basis-Umsatz │ 6 WKZ Basis-Umsatz │ 7 Konto │ 8 Gegenkonto (ohne BU-Schlüssel)
9 BU-Schlüssel │ 10 Belegdatum │ 11 Belegfeld 1 │ 12 Belegfeld 2 │ 13 Skonto │ 14 Buchungstext
```

A third-party export that names columns like `Umsatz`, `S/H`, `BU`, `Gegenkonto`, `Belegfeld`,
`Konto`, `KOST1`, `Buchungstext` is a Buchungsstapel-shaped file even when the vendor never uses
the word — hold it to this format.

**Formats and traps, all confirmed in the sample:**

| Thing | Value | Trap |
|-------|-------|------|
| Field separator | `;` | |
| Text delimiter | `"` | Text fields quoted, numerics bare |
| Decimal separator | **comma** — `100,18` | A period silently changes the amount or voids the row |
| Line endings | **CRLF** | |
| `Belegdatum` | **`3101` = DDMM only** | **The year is not in the field.** It comes from the header's fiscal year and date range. A booking outside `Datum von`–`Datum bis` lands in the wrong year — a silent, expensive class-3 failure |
| Encoding | **DATEV's contract: ISO-8859-1 / CP1252 by default.** UTF-8 is accepted **only with a BOM** | **Emit CP1252.** BOM-less UTF-8 is unambiguously wrong — DATEV reads it as CP1252 and mojibakes every umlaut. The "UTF-8 with a BOM is accepted" carve-out is widely repeated but is **not** stated in DATEV's own field description (Dok. 1003221 says ANSI), so do not treat it as a pass condition. DATEV's own 2025 sample is BOM-less UTF-8 and is therefore not a safe encoding template |
| `Soll/Haben-Kennzeichen` | `"S"` / `"H"` | Refers to **field 7 `Konto`**, not the Gegenkonto: `S = Soll (Belastung)`, `H = Haben (Gutschrift)` |
| `Umsatz` (field 1) | `100,18` | Must be positive with exactly 2 decimals, and **`0,00` is invalid**. A zero-amount line is a rejected row, not a no-op |
| `Belegfeld 1` (field 11) | invoice number, ≤ 36 chars | **Restricted charset**: word characters plus `$ & % * + - /`. **Spaces, umlauts, `.`, `,`, `;`, `:` are invalid.** Products that match payments on an invoice number write it here — if that number ever contains a space or a dot, this field is where it breaks |
| Account lengths | Sample: `Konto` `48400` (5 digits), `Gegenkonto` `8401` (4), `Sachkontenlänge` `4` | Personal accounts (Debitor/Kreditor) may be **exactly one digit longer** than Sachkonten. DATEV's only hard prohibition is an all-zero account number; short accounts need not be zero-padded. The real hazard is **Excel silently stripping leading zeros** on a round trip |
| `Festschreibung` | header field 21 **and** record field **114** | Two independent switches. An empty record field 114 means *undefined* → **auto-locked**. **If any single row carries `1`, the whole batch locks on import** — after which corrections are only possible as Generalumkehr/Storno |

### Chart of accounts — and a live finding

`SKR03` (ordered by business process) and `SKR04` (ordered by the statutory balance-sheet
structure) are the two general German frameworks. They **collide on the same numbers with opposite
meanings**:

| Number | In SKR03 | In SKR04 |
|--------|----------|----------|
| `1200` | Bank | Forderungen aus L+L |
| `1600` | Verbindlichkeiten aus L+L | Kasse |

Revenue is the mirror image — not a number collision but a **renumbering**, which is just as easy
to get wrong in the other direction:

| Concept | SKR03 | SKR04 |
|---------|-------|-------|
| Erlöse 19 % USt | `8400` | `4400` |

Note that `4400` is *not* a revenue account in SKR03 — class 4 there is Betriebliche Aufwendungen.
So a file that books revenue to `4400` while header field 27 declares SKR03 is booking revenue into
an expense account.

An export booked against the wrong SKR **imports cleanly and books everything wrong** — the
purest class-3 failure in this module. Header field 27 declares which chart is intended.

**The Verein-specific chart, and a question worth asking:** vendor copy in the German club-software
space still advertises `Standardkontenrahmen SKR 49`. **SKR49 is discontinued.** DATEV's succession
is documented:
`SKR42` has been available since fiscal year 2022 and replaces it; SKR49 was retired at the
2024/2025 year change (last new setup FY2023, last carry-forward FY2024), and **from 2025 only
SKR42 can be booked**.

A marketing page is not evidence of current product behaviour: field 27 is a per-client
configuration, and "the copy advertises a retired chart" is a different finding from "the export is
broken". Establish what field 27 actually carries before filing anything.

This matters beyond the label, because **the two charts separate tax spheres by completely
different mechanisms**:

- **SKR49 encoded the sphere in the account number** — the leading digit *was* the
  Tätigkeitsbereich (2 = ideeller Bereich, 3 = ergebnisneutrale Posten incl. erhaltene Spenden,
  4 = Vermögensverwaltung, 5 = Zweckbetrieb Sport, 6 = übrige Zweckbetriebe incl. Kultur,
  7/8 = steuerpflichtige Geschäftsbetriebe). Classes 3 and 6 are easy to forget and will
  mis-bucket donations and non-sport Zweckbetriebe on a legacy export.
- **SKR42 does not.** The sphere moves into **`KOST1` — record field 37**: `1` ideeller Bereich,
  `2` Vermögensverwaltung, `3` Zweckbetrieb, `4` wirtschaftlicher Geschäftsbetrieb, `9`
  Sammelposten.

**Consequence, and the highest-value DATEV test in this module:** for an SKR42 club, an income or
expense booking that leaves field 37 empty silently loses its sphere assignment — the entire
purpose of the chart — and the export still validates. A product whose export carries a `KOST1`
column and whose billing UI has cost centres plausibly has the wiring; **whether it is populated
per sphere is a per-product fact — establish it before filing.**

Two scoping caveats that must travel with this test, or it produces false positives:
**(a)** it applies to SKR42 clients only — on SKR03/SKR04 `KOST1` is legitimately optional, so
establish header field 27 before asserting anything; **(b)** sphere cost centres attach to income
and expense postings, not unconditionally to every line in the file.

*(Beware: third-party articles that describe SKR42 sphere assignment as account-number ranges are
wrong — that was SKR49's mechanism. The two are widely conflated in both directions.)*

**Why spheres exist at all.** German non-profit tax law splits a club's activity into four spheres
because only the last is fully taxable:

| Sphere | Typical club income | Taxed? |
|--------|---------------------|--------|
| Ideeller Bereich | Membership fees, joining fees, donations, grants | Exempt; no input-VAT deduction |
| Vermögensverwaltung | Interest, long-term letting, leased advertising rights | Exempt |
| Zweckbetrieb | Entry and start fees for sporting events, courses | Exempt |
| Wirtschaftlicher Geschäftsbetrieb | Club bar run in-house, food and drink, advertising and sponsoring | **Taxable** |

The threshold is a **Freigrenze, not an allowance**: exceed it by one euro and the *whole* profit
becomes taxable. It was raised to **50,000 €** (gross receipts, including VAT) effective
**1 January 2026** — as were the coach allowance (`Übungsleiterpauschale`) to **3,300 €** and the
volunteer allowance (`Ehrenamtspauschale`) to **960 €**. Sources still quoting 45,000 € / 3,000 € /
840 € are pre-2026.

Two rules that catch club software out: food-and-drink sales and advertising are forced **out** of
the sports-event bucket regardless of amount, and covering a commercial loss out of ideelle funds
endangers the club's charitable status outright. Mis-assigning a `Kostenstelle` is therefore not a
cosmetic defect.

## Gotchas

1. **`Belegdatum` has no year.** DATEV's DDMM plus a header period is a silent year-boundary trap.
   How DATEV disambiguates a month earlier than the fiscal-year start is **not documented** — do
   not guess; test it.
2. **Emit CP1252.** BOM-less UTF-8 mojibakes every umlaut; the "UTF-8 with a BOM" carve-out is not
   in DATEV's own field description, so do not rely on it. DATEV's own sample file is BOM-less
   UTF-8, so the sample is not a safe template for encoding.
3. **`EXTF` vs `DTVF`.** A third-party system must write `EXTF`, and the filename must match.
4. **`Festschreibung` is two fields, and one bad row locks the batch.** Record field 114 left
   empty means *auto-lock*, not *no lock*.
5. **SKR49 is discontinued; SKR42 puts the tax sphere in `KOST1`.** An SKR42 export whose income
   and expense rows omit `KOST1` loses the sphere split silently. Check header field 27 first —
   on SKR03/SKR04 `KOST1` is optional and asserting it produces false positives.
6. **DATEV's published field regexes do not match DATEV's own sample files** in at least six
   places (Datenkategorie, Formatname, Formatversion, `BU-Schlüssel`, `Sachkontenrahmen`,
   Generalumkehr). Do not implement the regexes as literal validators — use the Prüfprogramm.
7. **The 2026 statutory figures changed.** 50,000 € Freigrenze, 3,300 € coach allowance, 960 €
   volunteer allowance. Anything quoting 45,000 / 3,000 / 840 is stale.

## References

Retrieved 2026-08-08. DATEV documents are copyrighted — this module describes and cites them; it
reproduces no specification text.

| Source | Reference |
|--------|-----------|
| DATEV format documentation, Musterdaten bundle, and the free *Prüfprogramm DATEV-Format* v2.2.3.0 | `https://developer.datev.de` — sample `EXTF_Buchungsstapel.csv` dated 2025-06-18 |
| DATEV help documents | 1044208 (why a file is not offered for import), 1080697 (Festschreibung), 1031527 (SKR49 → SKR42 succession), 1022996 (SKR42 sphere cost centres) |
| SKR42 / SKR49 chart definitions | DATEV Art.-Nr. 12901/12902 (SKR42), 11180 (SKR49, discontinued) |
| 2026 statutory figures | Steueränderungsgesetz 2025 — §§ 64, 67a AO (50,000 €), § 3 Nr. 26 / 26a EStG (3,300 € / 960 €) |
