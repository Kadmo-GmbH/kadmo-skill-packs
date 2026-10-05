---
name: pain.008 & Mandate Rules
path: /sepa-direct-debit
tags: ["@qa", "@billing", "@sepa", "@german"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.8
needs_reverification: false
notes: "Specification layer verified against primary EPC sources (EPC016-06 / EPC130-08 2025 editions, retrieved and text-extracted 2026-08-08); MOD-97-10 and Creditor-Identifier check-digit behaviour verified locally against published examples. Extracted into this community pack 2026-08-12."
---

# SEPA Direct Debit — pain.008 & Mandate Rules

## Overview

The format layer of SEPA direct-debit QA: the pain.008 message and its character rules, the mandate
lifecycle and sequence types, collection timing, CORE vs B2B, R-transaction reason codes, and
IBAN/Creditor-Identifier validation. It exists so a QA report can say *which field is wrong and
which rule it violates* — with the EPC index number — rather than "the button did not error".

The operational layer — failure taxonomy, synthetic fixture matrix, and the run-them-all validation
heuristics — lives in this pack's [root](../_skill.md); the accounting-export half is
[datev-extf](../datev-extf/_skill.md). Version pinning matters here: a rule from the wrong scheme
or the wrong year gives money-path QA false confidence, which is worse than no module.

## Specification Sources — Pinned

Version pinning matters here: a rule from the wrong scheme or the wrong year gives money-path QA
false confidence, which is worse than no module. All retrieved and text-extracted 2026-08-08.

| Document | ID / version | Effective | What it governs |
|----------|--------------|-----------|-----------------|
| SDD **Core** Scheme Rulebook | `EPC016-06`, 2025 **v1.1** | 05 Oct 2025 | Timing, mandate rules, refund rights |
| SDD Core **Customer-to-PSP Implementation Guidelines** | `EPC130-08`, 2025 v1.0 | 05 Oct 2025 | The pain.008 file a creditor sends its bank |
| SDD **B2B** Rulebook | `EPC222-07`, 2025 **v1.1** | 05 Oct 2025 | B2B scheme differences |
| SDD B2B Customer-to-PSP IG | `EPC131-08`, 2023 v1.1 | 17 Mar 2024 | B2B message mapping |
| Clarification Paper, SDD Core & B2B | `EPC132-17` | — | Edge-case readings |

**Message version: `pain.008.001.08`.** The Core IG states it implements the **2019 ISO 20022
message version**; the B2B IG maps the same `Customer Direct Debit Initiation V08`. A club still
emitting `pain.008.001.02` is on the pre-2023-migration version — flag it, do not assume the bank
still accepts it.

> `EPC135-18 "Guidance on Reason Codes"` covers **SCT (credit transfers), not direct debits.** It
> is easy to find and easy to cite by mistake. Use the SDD IG for direct-debit reason codes.

## pain.008 — Direct Debit Initiation

Three nested levels. Getting the level right is most of the job — a field checked at the wrong
level is the most common false bug report.

```
Document
└── CstmrDrctDbtInitn
    ├── GrpHdr                     ← once per file: who sent it, when, how many, total
    └── PmtInf  (1..n)             ← one per (sequence type × due date × creditor account)
        ├── creditor + collection date + sequence type
        └── DrctDbtTxInf  (1..n)   ← one per member being collected from
            └── DrctDbtTx → MndtRltdInf   ← the mandate data
```

Element index, with cardinality, the Rulebook attribute it carries, and the EPC usage rule.
Taken from `EPC130-08` (index numbers are the IG's own):

| IG § | Card. | XML tag | Level | AT | Rule / note |
|------|-------|---------|-------|----|-------------|
| 1.1 | 1..1 | `MsgId` | GrpHdr | — | Sender's unique file id |
| 1.2 | 1..1 | `CreDtTm` | GrpHdr | — | File creation timestamp |
| 1.4 | 1..1 | `NbOfTxs` | GrpHdr | — | Must equal the actual transaction count |
| 1.5 | 1..1 | `CtrlSum` | GrpHdr | — | **Mandatory in SEPA** (optional in base ISO). Must equal the sum of `InstdAmt` |
| 2.1 | 1..1 | `PmtInfId` | PmtInf | — | |
| 2.2 | 1..1 | `PmtMtd` | PmtInf | — | `DD` |
| 2.3 | 0..1 | `BtchBookg` | PmtInf | — | `true` ⇒ the bank books one aggregate entry (German banking UIs: `Mengenbuchung`) |
| 2.4 | 1..1 | `NbOfTxs` | PmtInf | — | **Mandatory, and separate from 1.4.** Count *within this `PmtInf` block* |
| 2.5 | 1..1 | `CtrlSum` | PmtInf | — | **Mandatory, and separate from 1.5.** Sum *within this `PmtInf` block* |
| 2.14 | 1..1 | `SeqTp` | PmtInf | AT-M006 | **Mandatory.** `FRST` / `RCUR` / `OOFF` / `FNAL` |
| 2.18 | 1..1 | `ReqdColltnDt` | PmtInf | AT-T013 | Due date (German banking UIs: `Einzugsdatum`) |
| 2.68 | 0..1 | `ChrgBr` | PmtInf | — | **Only `SLEV` is allowed** |
| 2.71 | 0..1 | `CdtrSchmeId` | PmtInf | — | Creditor Identifier (Gläubiger-ID). IG recommends all transactions **within one `PmtInf` block** share it, and requires it at **either** `PmtInf` **or** `DrctDbtTxInf` level (2.133) — so absence here is not a defect if it is present per transaction |
| 2.84 | 1..1 | `EndToEndId` | DrctDbtTxInf | AT-T015 | Reference returned unchanged on an R-transaction — **the join key for reconciliation** |
| 2.98 | 1..1 | `InstdAmt` | DrctDbtTxInf | AT-T002 | **Only `EUR`**; SEPA inclusive range **0.01 .. 999999999.99** — a `0.00` or negative amount is invalid |
| 2.102 | 1..1 | `MndtId` | MndtRltdInf | AT-M001 | **Mandatory.** Length 1..35 |
| 2.103 | 1..1 | `DtOfSgntr` | MndtRltdInf | AT-M008 | **Mandatory.** Mandate signature date |
| 2.104 | 0..1 | `AmdmntInd` | MndtRltdInf | — | `true` ⇒ `AmdmntInfDtls` becomes mandatory |
| 2.206 | 0..1 | `RmtInf` | DrctDbtTxInf | AT-T012 | Remittance information. Either `Strd` or `Ustrd` may be present |
| 2.207 | 0..1 | `Ustrd` | RmtInf | — | Unstructured remittance info shown to the debtor; only one occurrence allowed. (`AT-E001` is the *Creditor's name*, not this element — 2.207 only references it when a trade name differs from the legal name) |

### The character set — the rule most often broken

The XML is UTF-8, but SEPA only guarantees support for this Latin subset:

```
a–z  A–Z  0–9  /  -  ?  :  (  )  .  ,  '  +  and space
```

And **references, identifications and identifiers** (`MsgId`, `PmtInfId`, `EndToEndId`, `MndtId`)
carry three extra constraints: restricted to that set, must not begin or end with `/`, and must
not contain `//`.

German member names contain `ä ö ü ß`, and `ß`'s capital `ẞ` plus imported names carry more. This
is not theoretical: membership-software changelogs are full of fixes sanitising `ẞ` and `ą` after
an `XML-Fehler beim Bank-Upload`. Transliteration (`ä→ae`, `ß→ss`) in names is acceptable
and normal; an un-transliterated character in a *reference* field is a defect.

## Mandate Lifecycle

A mandate (`SEPA-Lastschriftmandat`) is the debtor's standing authorisation. Its identity is the
pair **`MndtId` + `CdtrSchmeId`** — the creditor's own reference plus the Gläubiger-ID.

**Sequence types** (`SeqTp`, AT-M006):

| Code | Meaning | When |
|------|---------|------|
| `FRST` | First | First collection of a recurring series |
| `RCUR` | Recurrent | Every subsequent collection |
| `OOFF` | One-off | A single collection; the mandate is spent afterwards and must not be reused |
| `FNAL` | Final | Last collection of a series |

**`FRST` is no longer mandatory.** Rulebook v9.0 (approved 11 Dec 2014, **effective November
2016**) allows a first collection to be sent as `RCUR`. So:

- `RCUR` on a first-ever collection is **correct**, not a bug.
- `FRST` → `RCUR` is correct.
- `RCUR` → `FRST` on the same mandate is a defect **only on an unamended mandate**. IG 2.14 carves
  out the amendment case explicitly: if `AmdmntInd` is `true` **and** `OrgnlDbtrAcct` is set to
  `SMNDA`, the element "indicates either `FRST`, `RCUR`, `FNAL` or `OOFF` (all four codes allowed,
  no restrictions)". Check `AmdmntInd` before reporting a sequence-type regression — this is
  amendment territory (fixture 7 in the [pack root](../_skill.md)'s matrix), and reporting it blind is a false positive.
- `OOFF` used twice on one mandate is a defect.
- After `FNAL`, further collections on that mandate are a defect.

**Amendments.** Set `AmdmntInd` = `true` and supply `AmdmntInfDtls`, using the element that matches
what changed — the IG makes each conditional on that specific change:

| Changed | IG § | Element | IG rule |
|---------|------|---------|---------|
| Mandate reference | 2.106 | `OrgnlMndtId` | Mandatory if `MndtId` changed, otherwise must not be used |
| Creditor identity / Gläubiger-ID | 2.107 | `OrgnlCdtrSchmeId` | Mandatory if `CdtrSchmeId` and/or `Name` changed, otherwise not to be used |
| Debtor's account, **same** PSP | 2.120 | `OrgnlDbtrAcct` | The old IBAN is allowed in this case |
| Debtor's account at a **new** PSP | 2.120 | `OrgnlDbtrAcct` = `SMNDA` | `Othr/Id` = `SMNDA` (*Same Mandate with a New Debtor Account*) |

> **`OrgnlDbtrAgt` is a trap in `pain.008.001.08`.** IG 2.121 states it is **"not to be used if
> element 'Original Debtor Account' is populated with `SMNDA`"** — so a change of debtor *bank* is
> signalled through `OrgnlDbtrAcct`/`SMNDA` (2.120), **not** by populating `OrgnlDbtrAgt`. Putting
> the new bank in `OrgnlDbtrAgt` is the older `pain.008.001.02` habit and is now prohibited in the
> SMNDA case. Score a "new bank" amendment against 2.120, never against 2.121.

A creditor that silently issues a **new** mandate instead of amending an existing one loses the
mandate's history — a real defect that no syntax validator catches.

**36-month dormancy.** If no collection is presented under a mandate for 36 months (counted from
the last collection), the mandate expires and the creditor must cancel it. Collecting under a
dormant mandate invites reason code `MD01` (NoMandate). Seasonal clubs — a sports Verein whose
members pause — are exactly the population this rule bites.

**Gläubiger-ID (Creditor Identifier).** 18 characters for Germany:

```
DE 98 ZZZ 09999999999
│  │  │   └── 11-char national identifier (issued by the Bundesbank)
│  │  └────── 3-char business code (Geschäftsbereichskennung), freely chosen by the creditor
│  └───────── 2 check digits
└──────────── country code
```

Check digits validate with the same MOD-97-10 as an IBAN over *(national identifier + country code
+ check digits)*. **The 3-char business code is excluded from the calculation** — verified
empirically: swapping `ZZZ` → `ABC` → `001` leaves the identifier valid. Consequence for QA: a
club can run several business codes off one Gläubiger-ID, and a mutated business code will **not**
be caught by a check-digit validator.

## Collection Timing & Pre-Notification

From the Core Rulebook (`EPC016-06` 2025 v1.1):

| Rule | Value |
|------|-------|
| Pre-notification to the debtor | At the latest **14 calendar days** before the due date, unless the creditor and debtor agreed another timeline |
| Presentation to the creditor's bank | Up to **D-1** inter-bank business day before the due date — for **all** sequence types since the November 2016 rulebook |
| Unconditional refund ("no questions asked") | **8 weeks** from the debit date |
| Claim for an unauthorised collection | Up to **13 months** after the debit date |

A pre-notification is any advance notice naming the amount and the due date — an invoice e-mail
qualifies. **The 14-day rule is a compliance obligation with no technical enforcement**: nothing in
the XML encodes it, no bank rejects a file for breaching it, and debtors complain instead. If the
product under test lets the invoice-send date and the collection date be set less than 14 days
apart without a warning, that is a legitimate finding even though every file validates.

The `D-5` lead time for `FRST` that older material still describes was abolished in November 2016.
Do not report a `FRST` sent at D-1 as a defect.

## CORE vs B2B

| | CORE (`pain.008`, SDD Core) | B2B (SDD B2B) |
|---|---|---|
| Debtor | Anyone, including consumers | **Must not be a consumer** — the debtor's bank is obliged to ensure this |
| Refund for an *authorised* collection | 8 weeks, no questions asked | **None.** The rulebook states the debtor has no right to obtain a refund for an authorised transaction |
| Debtor bank checks the mandate | No | **Yes** — obliged to check mandate status before paying |

A Verein billing private members is a CORE scheme user. B2B is relevant only for corporate
sponsors or facility renters. Applying a B2B assumption (no refund) to a consumer collection is a
serious error — the 8-week right stands.

## Bank Statement Reconciliation

Many German membership and club products reconcile payments from a **plain CSV exported from the
organisation's own online banking**, matching an invoice number found in the `Verwendungszweck`
(remittance text). If the product under test works this way, the CSV path is cheaper to break than
the XML path, and it deserves its own fixtures:

- The CSV has **no schema**. Column naming (`Betrag`, `Umsatz`, …), ordering, encoding, decimal
  comma, and date format all vary between German banks — build fixtures from **several** banks'
  export dialects, not one.
- Free-text `Verwendungszweck` matching fails when a bank truncates the field, when a payer edits a
  transfer reference, or when two invoice numbers appear in one line.
- A returned debit (`Rücklastschrift`) in the statement must reopen or hold the invoice and carry
  the return fee — verify the product's documented behaviour rather than assuming either.

**Background — camt, if the product ever adopts it.** `camt.053` is the end-of-day statement;
`camt.054` is a debit/credit notification. Both nest `Stmt`/`Ntry` (booked entry) → `NtryDtls` →
`TxDtls` (individual transaction), where a return carries the original `EndToEndId` plus a reason
code — which is why `EndToEndId` is the reconciliation join key. `MT940` is the older flat-file
equivalent.

## R-Transactions

An R-transaction is any collection that does not complete normally. Ordered by when they surface:

| Type | Who raises it | When | Effect |
|------|---------------|------|--------|
| **Reject** | Creditor bank, CSM, or debtor bank | Before settlement | Never settles |
| **Refusal** | Debtor | Before settlement | Debtor bank rejects the collection |
| **Return** | Debtor bank | After settlement, within 5 inter-bank business days | Funds reversed |
| **Refund** | Debtor | Within **8 weeks** (13 months if unauthorised) | Funds returned to debtor |
| **Reversal** | Creditor | After settlement | Creditor voluntarily repays a collection it should not have made |
| **Revocation / Request for cancellation** | Creditor | Before settlement | Creditor withdraws the collection |

**Reason codes**, verified against the SDD Core IG (`EPC130-08`). This is the SDD list — not the
SCT list:

| Code | ISO name | Reading |
|------|----------|---------|
| `AC01` | IncorrectAccountNumber | Account identifier wrong |
| `AC04` | ClosedAccountNumber | Account closed |
| `AC06` | BlockedAccount | Account blocked |
| `AG01` | TransactionForbidden | Direct debit not permitted on this account |
| `AG02` | InvalidBankOperationCode | Operation/transaction code invalid |
| `AM04` | InsufficientFunds | **The ordinary one.** Not a software defect |
| `AM05` | Duplication | Duplicate entry — **a real billing-run bug**: the same collection went out twice |
| `BE01` | InconsistentWithEndCustomer | **Debtor's name does not match the account holder's name.** The classic membership-billing cases: a minor billed through a guardian, or a family/group account where the payer owns the account but a managed member's name was sent |
| `BE05` | UnrecognisedInitiatingParty | Creditor identifier wrong — check `CdtrSchmeId` |
| `CNOR` / `DNOR` | CreditorBankIsNotRegistered / DebtorBankIsNotRegistered | The creditor's or debtor's PSP is not registered under this scheme |
| `FF01` | InvalidFileFormat | Format defect |
| `MD01` | NoMandate | No valid mandate — dormancy, wrong `MndtId`, or a revoked mandate |
| `MD02` | MissingMandatoryInfomationInMandate | Mandate data missing or wrong |
| `MD07` | EndCustomerDeceased | Debtor deceased |
| `MS02` | NotSpecifiedReasonCustomerGenerated | **Refusal by the Debtor** — the member actively refused the collection. Not "no reason given"; this is an actionable member event, not noise |
| `MS03` | NotSpecifiedReasonAgentGenerated | Reason not specified (agent-generated) |
| `RC01` | BankIdentifierIncorrect | Bank identifier invalid |
| `RR01`–`RR03` | MissingDebtorAccountOrIdentification / MissingDebtorNameOrAddress / MissingCreditorNameOrAddress | The specific missing-data regulatory reasons |
| `RR04` | RegulatoryReason | **Generic** regulatory reason — not a missing-field code |
| `SL01` | SpecificServiceOfferedByDebtorAgent | Debtor's bank service, e.g. a debit block or a whitelist |

`MD02`'s ISO name really is spelled **`MissingMandatoryInfomationInMandate`** — missing the `r`.
A validator matching on the name string must use the spec's spelling.

`MD06` (refund requested by the end customer) is **not** in the customer-to-PSP list; it belongs to
the inter-bank refund space. Its absence from a pain.008 file is correct.

Triage rule: `AM04`, `MD07` and `MS02` are the member's world — insufficient funds, a death, and a
deliberate refusal respectively. `AM05`, `BE01`, `BE05`, `MD01`, `MD02` and `FF01` point at the
billing run and deserve a ticket.

> Scope note: the EPC130-08 table this list is drawn from enumerates the reasons for a **Reject**
> by the Creditor PSP, the Debtor PSP or the CSM. Returns, refunds and refusals reuse the same code
> values but are governed by the Rulebook and the inter-PSP guidance — for the SDD R-transaction
> guidance proper, the document is `EPC173-14`, **not** the SCT paper named in gotcha 12.

## IBAN & BIC Validation

**MOD-97-10 (ISO 7064)** — verified locally against published example IBANs:

1. Move the first four characters to the end.
2. Replace each letter with two digits, `A`=10 … `Z`=35.
3. Interpret as an integer. **Valid iff the remainder mod 97 equals 1.**

German IBANs are **22 characters**: `DE` + 2 check digits + 8-digit BLZ + 10-digit account number,
left-padded with zeros.

Verified locally: this catches a changed digit, a transposition of two *different* digits, and any
length error. It does **not** catch a transposition of two identical digits, and it does not tell
you the account exists.

**BIC is not required** for SEPA payments within the EEA — the "IBAN-only" rule, which comes from
the **SEPA Regulation (EU) 260/2012**, not from the EPC scheme documents (national payments from
February 2014, cross-border from February 2016). A missing `CdtrAgt`/`DbtrAgt` BIC is therefore not
automatically a defect; check the bank's own requirement before reporting one. Where a BIC *is*
present, the IG accepts either the 8- or the 11-character form.

One detail worth knowing when comparing creditor identifiers: the IG declares the Creditor
Identifier data element **case- and space-insensitive**, so `DE98ZZZ09999999999` and
`de98 zzz 099 999 999 99` are the same identifier. A validator that compares them as raw strings
will report false mismatches.

## Gotchas

1. **`FRST` is not required.** The commonest false positive in SEPA QA. `RCUR` on a first
   collection has been valid since November 2016.
2. **`CtrlSum` is mandatory in SEPA** though optional in base ISO 20022 — generic XML tooling will
   not flag its absence.
3. **Umlauts are fine in names, fatal in references.** Different rules for different fields — and
   `Belegfeld 1` on the DATEV side ([datev-extf](../datev-extf/_skill.md)) is stricter still: no
   spaces, no dots.
4. **The Creditor-Identifier business code is not check-digit protected.** Corruption there passes
   every validator.
5. **The 14-day pre-notification has no technical enforcement.** It will never fail a file.
6. **Don't cite the SCT reason-code guidance** (`EPC135-18`) for direct debits.
7. **B2B has no refund right; CORE has 8 weeks.** Never carry a B2B assumption into a consumer
   collection.
8. **Most German membership products import bank statements as CSV, not camt.** Test the dialect
   problem before the ISO one — see *Bank Statement Reconciliation*.

## References

Specifications retrieved and text-extracted 2026-08-08. EPC documents are copyrighted — this module
describes and cites them; it reproduces no specification text.

| Source | Reference |
|--------|-----------|
| SDD Core Scheme Rulebook | `EPC016-06`, 2025 v1.1, effective 05 Oct 2025 |
| SDD Core Customer-to-PSP Implementation Guidelines | `EPC130-08`, 2025 v1.0, effective 05 Oct 2025 |
| SDD B2B Scheme Rulebook | `EPC222-07`, 2025 v1.1, effective 05 Oct 2025 |
| SDD B2B Customer-to-PSP Implementation Guidelines | `EPC131-08`, 2023 v1.1, effective 17 Mar 2024 |
| Clarification Paper on SDD Core and B2B Rulebooks | `EPC132-17` |
| European Payments Council | `https://www.europeanpaymentscouncil.eu` |
| SEPA Regulation (EU) 260/2012 | The "IBAN-only" rule — a regulation, not an EPC document |
| ISO 7064 MOD-97-10 | Verified locally against published example IBANs |
