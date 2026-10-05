---
name: SEPA & DATEV Rails
domain: sepa-datev
path: /
tags: ["@qa", "@billing", "@sepa", "@datev", "@german"]
learned: "2026-08-08"
last_verified: "2026-08-12"
confidence: 0.7
---

# SEPA Direct Debit & DATEV Export

Fee billing is the highest-trust surface of any German membership product. A billing run that
reports success but emits a malformed file is a **failed** run — the failure just surfaces later,
at the bank, in someone else's accounting system. This pack supplies the format knowledge needed to
say *which field is wrong and which rule it violates*, rather than "the button did not error".

Everything here is specification-layer knowledge, verified against primary EPC and DATEV sources —
what any German-market billing product must emit. What a *specific* product actually does (its
export dialects, its status vocabulary, its configuration) belongs in that product's own domain
pack, layered on top of this one.

## Module Inventory

| Module | Covers | `_skill.md` |
|--------|--------|:-----------:|
| [sepa-direct-debit](sepa-direct-debit/_skill.md) | pain.008 structure and character rules, mandate lifecycle, timing, CORE vs B2B, R-transaction reason codes, IBAN/Creditor-ID validation, bank-statement reconciliation | 0.8 |
| [datev-extf](datev-extf/_skill.md) | EXTF/Buchungsstapel header and record format, silent-non-import rules, SKR03/04/42/49 charts, non-profit tax spheres and `KOST1` | 0.75 |

The failure taxonomy, fixture matrix and validation heuristics below span both modules — run the
whole heuristics list on every export; an XML failure must never suppress the DATEV checks.

## Failure Taxonomy

Ordered by how late the failure surfaces. Later is worse: the cost of a defect here is roughly the
time until someone notices.

| # | Class | Example | Detected by |
|---|-------|---------|-------------|
| 1 | Rejected by schema | Missing `MndtId`, wrong namespace, `NbOfTxs` ≠ actual count | XSD validation — free, run it first |
| 2 | Rejected by the bank | `ChrgBr` ≠ `SLEV`, non-EUR `InstdAmt`, illegal character in a reference, `CtrlSum` mismatch | Bank upload — the club sees an error and calls support |
| 3 | **Accepted, books wrongly** | Right amount to the wrong member; `EndToEndId` not unique; wrong `ReqdColltnDt`; `BtchBookg` flipped so the statement shows one lump | Only by reading the file — **this is the class this module exists for** |
| 4 | Correct file, wrong business rule | Pre-notification under 14 days; collecting from a member who resigned; double-billing a family-group payer | Business-rule assertions over the run |
| 5 | Surfaces weeks later | `MD01` from a dormant mandate; `AM05` duplicates; refunds inside the 8-week window | Return file — after the money moved |

A QA report is useful in proportion to how high up this list it can push a defect.

## Test Fixture Matrix

**Synthetic only.** Every value below was generated or is a published documentation example; none
is a real account. Never copy a value out of a real screen, statement, or export.

**Verified-valid IBANs** (each passes MOD-97-10, checked locally):

```
DE50100000000000000001      DE23100000000000000002
DE41500105170123456789      DE62370400440532013001
DE72120300000000202052
```

**Must-be-rejected IBANs** (each verified to fail):

| Value | Defect |
|-------|--------|
| `DE00500105170123456789` | Check digits zeroed |
| `DE41500105170132456789` | Two account digits transposed |
| `DE41500105170123456780` | Single digit changed |
| `DE4150010517012345678` | 21 characters |
| `DE415001051701234567890` | 23 characters |
| `de41500105170123456789` | Lower-case. **Note: this one passes MOD-97-10** (uppercase it and the remainder is 1) — it must be rejected on the ISO 13616 upper-case rule, which is why assertion 6 states the case rule separately. A validator that silently upper-cases its input will pass this fixture; that is the defect it is testing for |

**Verified-valid Gläubiger-IDs:** `DE13ZZZ00000000123`, `DE52ZZZ00012345678`,
`DE98ZZZ09999999999`. Rejected: `DE98ZZZ09999999998` (national identifier mutated).

**Member and run cases** — each maps to a failure class above:

| # | Fixture | Targets |
|---|---------|---------|
| 1 | Names with `ä ö ü ß`, capital `ẞ`, and a non-German diacritic (`ą`, `ł`) | Character-set rules — names transliterate, references reject |
| 2 | A name long enough to overflow `Ustrd` / `Buchungstext` | Truncation |
| 3 | First-ever collection sent as `RCUR` | Must **pass** — `FRST` is not mandatory |
| 4 | `FRST` then `RCUR` on one mandate | Correct transition |
| 5 | `RCUR` after `FNAL`, and `OOFF` reused | Must **fail** |
| 6 | Mandate with no collection for 36 months | Dormancy → `MD01` |
| 7 | Mandate amended: new IBAN same bank; new bank; changed `MndtId` | `AmdmntInd` + the matching `Orgnl*` element |
| 8 | Minor billed through a guardian (`Erziehungsberechtigte`) | Debtor is the guardian, member is the child — names must not be swapped |
| 9 | Family/group account with one payer and three managed members | One debtor, correct total, no double billing |
| 10 | Mid-year fee change and an early-ended fee | Pro-rating and refund handling |
| 11 | Two invoice numbers in one `Verwendungszweck`; a truncated one | CSV reconciliation |
| 12 | `Rücklastschrift` (returned debit) in the uploaded statement | Invoice reopens/holds with the return fee attached — per the product's documented behaviour |
| 13 | Year-end run: `Belegdatum` in December, header fiscal year the next | The DDMM year trap |
| 14 | Booking dated outside `Datum von`–`Datum bis` | Header/period consistency |
| 15 | Run with zero collectable members | `NbOfTxs` = 0 handling, empty-file behaviour |
| 16 | Amount `0,00`, `-1,00`, and one above 999999999.99 | `InstdAmt` inclusive range 0.01..999999999.99 (**SEPA rejects `0.00` too**, IG 2.98); DATEV rejects `0,00` in `Umsatz` |
| 17 | Two members with identical names and amounts | `EndToEndId` uniqueness |
| 18 | A collection date under 14 days after invoice send | Pre-notification rule |
| 19 | A `Rechnungsnummer` containing a space, a dot, or an umlaut | DATEV `Belegfeld 1` charset |
| 20 | Bookings across all four tax spheres in one export | `KOST1` populated per row (SKR42) |
| 21 | A batch at the row boundary — 99,996 / 99,997 / 99,998 postings | DATEV per-file row limit (99,999 rows incl. summary lines) |

## Validation Heuristics — Run These On An Export

Assertions in order of cost. Each is cheap and **independent — run them all and report every
failure**. Do not stop at the first: the two blocks below cover two different artefacts (1–13 the
`pain.008` XML, 14–19 the DATEV file), so an XML failure must never suppress the DATEV checks.

| # | Check | Rule source |
|---|-------|-------------|
| 1 | Validates against the XSD for the version the organisation is configured to emit — `pain.008.001.08` unless the product's export-format selector says otherwise | IG `EPC130-08` |
| 2 | `NbOfTxs` equals the count of `DrctDbtTxInf` — **at group level and again within every `PmtInf`** | IG 1.4 and IG 2.4 |
| 3 | `CtrlSum` equals the sum of `InstdAmt` — **at group level and again within every `PmtInf`** | IG 1.5 and IG 2.5, both mandatory in SEPA |
| 4 | Every `InstdAmt` has `Ccy="EUR"` and lies in **0.01 .. 999999999.99** inclusive | IG 2.98 |
| 5 | `ChrgBr` absent or `SLEV` | IG 2.68 |
| 6 | Every IBAN is **upper-case** (ISO 13616), passes MOD-97-10, and German ones are 22 chars | ISO 13616 + ISO 7064 |
| 7 | `CdtrSchmeId` passes the Creditor-Identifier check | Verified locally |
| 8 | `MndtId` and `EndToEndId`: ≤ 35 chars, allowed charset only, no leading/trailing `/`, no `//` | Length: IG 2.102 / 2.84. Charset and the `/` rules: the IG character-set rule, not an element index — see `sepa-direct-debit` § *The character set* |
| 9 | `EndToEndId` unique within the file | Reconciliation |
| 10 | `SeqTp` present, and consistent with that mandate's history | IG 2.14 + rulebook |
| 11 | `DtOfSgntr` present, not in the future, not after `ReqdColltnDt` | IG 2.103 |
| 12 | Where `AmdmntInd` is `true`, `AmdmntInfDtls` is present and carries the element matching what changed | IG 2.105 (`AmdmntInfDtls`), 2.106 (`OrgnlMndtId`), 2.107 (`OrgnlCdtrSchmeId`), 2.120 (`OrgnlDbtrAcct`/`SMNDA`); and 2.121 `OrgnlDbtrAgt` **absent** whenever 2.120 is `SMNDA` |
| 13 | `Sachkontenlänge` in the header matches the target client's DATEV configuration; no all-zero account number. **Personal accounts (Debitor/Kreditor) are legitimately one digit longer than Sachkonten, and short accounts need not be zero-padded** — do not flag either | DATEV field spec (see the account-length row above) |
| 14 | DATEV: line 1 has 31 fields, line 2 has 125, `EXTF` not `DTVF`, field 4 agrees with field 3, filename `EXTF_*.csv` | DATEV sample + doc 1044208 |
| 15 | DATEV: every amount uses a decimal comma, no `0,00` in `Umsatz`; every `Belegdatum` is 4 digits and inside the header period | DATEV field spec |
| 16 | DATEV: `Belegfeld 1` contains no space, umlaut, `.`, `,`, `;` or `:` | DATEV field spec |
| 17 | DATEV: on an **SKR42** client, income and expense rows carry `KOST1` (field 37) so the tax sphere is not lost. **Establish the client's chart first** — on SKR03/SKR04 `KOST1` is legitimately optional, and whether the exporter populates it per sphere is a per-product fact | SKR42 sphere model |
| 18 | DATEV: file is **CP1252 / ISO-8859-1**. Treat any UTF-8 as a finding pending confirmation — DATEV's documented Zeichensatz for a Buchungsstapel is ANSI, and the BOM carve-out is not stated in DATEV's own field description | DATEV Zeichensatz rule |
| 19 | Run the file through DATEV's free *Prüfprogramm DATEV-Format* before reporting any structural DATEV defect | DATEV Prüfprogramm |

**Not checkable on the export — verify out of band.** `ReqdColltnDt` ≥ 14 calendar days after the
pre-notification (Rulebook `EPC016-06`) is a genuine compliance rule, but the pre-notification date
is **not in the artefact**, so it cannot be an assertion over the file (see
`sepa-direct-debit` § *Collection Timing & Pre-Notification*). Check it against the
invoice-send date in the run, and remember the Rulebook allows another timeline if creditor and
debtor agreed one — a club's fee bylaws (Beitragsordnung) routinely do.

**Use the official validator first.** DATEV publishes a free *Prüfprogramm DATEV-Format*
(v2.2.3.0) and a Musterdaten bundle on `developer.datev.de`, neither requiring a login. Running an
export through it costs nothing and settles the structural questions before any manual review —
make it the first step of any DATEV ticket.

## Authoring Rules

1. **Synthetic fixtures only, forever.** Every member, mandate, IBAN and account in this pack was
   generated or is a published documentation example. Never add a value copied from a real screen,
   statement, or export.
2. **Pin the specification edition.** Every rule cites its document and version; a rule from the
   wrong year gives money-path QA false confidence, which is worse than no rule.
3. **No product-specific claims.** What one vendor's exporter does belongs in that consumer's own
   domain pack, never here.
