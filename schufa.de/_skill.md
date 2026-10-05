---
name: Schufa Credit Bureau
domain: schufa.de
path: /
tags: ["@finance", "@credit-score", "@germany", "@self-service", "@bonität"]
learned: "2026-05-11"
last_verified: "2026-05-11"
confidence: 0.85
---

# Schufa Credit Bureau

Schufa Holding AG is Germany's largest consumer credit bureau. The consumer-facing self-service portal at `app.schufa.de` lets account holders view their credit score, browse stored contracts and inquiries, manage identity protection, and request data corrections. This pack documents the structure of the portal and — most importantly — the **2026 12-factor scoring methodology** that replaced the previous opaque scoring model on **17 March 2026**.

The marketing/info site at `www.schufa.de` is a separate property not covered by this pack.

## What This Is Not

Schufa is a private credit bureau, not a bank. The portal does not let you transfer money, open accounts, or take out loans. It is a read-only consumer dashboard plus a data-correction request form. Score recalculation is not on-demand — it happens **once per quarter** automatically.

## Authentication

| Property | Value |
|---|---|
| Login URL | `https://www.meineschufa.de/de/login` → redirects into `app.schufa.de` |
| Method | Email + password + 2FA (SMS or authenticator app) |
| Session | Browser cookie; expires on close or idle timeout |
| SSO | None |
| Free annual data copy | Available separately via `meineschufa.de` → "Datenkopie nach Art. 15 DSGVO" (NOT the paid product) |

Agents cannot log in autonomously without stored credentials and access to the 2FA channel. Treat Schufa as a session that must be hand-bootstrapped by the human, then handed off.

## Global Navigation

The portal has four top-level tabs, always visible in a sticky banner:

| Tab | URL | Purpose |
|---|---|---|
| Start | `/` | Dashboard / greeting, entry points to score and entries |
| Score & Bonität | `/bonidata` | Score overview, 12-factor breakdown, entries summary, personal data |
| Identitätsschutz | `/identitaetsschutz` (path tentative) | Data-leak monitoring service |
| Service | `/service` (path tentative) | Help, data correction requests, contact forms |

The banner also contains a profile chip (top-right) showing the user's initials and linking to account settings.

## Score Update Cadence

The Schufa score recalculates **every 3 months**. The portal displays the calculation date prominently (e.g., "Ermittelt am: DD.MM.YYYY"). Tier crossings (e.g., address aging from "3 years" to "4 years") only take effect at the next recalculation, not on the day the tier is crossed.

## 2026 Score Reform Summary

The reform of 17 March 2026 replaced the legacy black-box model with a transparent **12-factor point system** scaled from 100 to 999. Each factor contributes a fixed maximum number of points; the total is the consumer's score.

| Band | Score | Approximate share of population |
|---|---|---|
| Hervorragend (Excellent) | 776–999 | varies |
| Gut (Good) | 709–775 | ~20% |
| Akzeptabel (Acceptable) | 642–708 | |
| Ausreichend (Sufficient) | 100–641 | |
| Ungenügend (kein Score) | — (insufficient data) | |

The 12 factors (full tier tables and methodology live in the `score/` module):

| # | Factor | Max P | Type |
|---|---|---:|---|
| 1 | Keine Zahlungsstörung (no payment defaults) | 264 | History |
| 2 | Keine neuen Konto- & Kreditkartenanfragen (last 12mo) | 117 | Recency |
| 3 | Weitere Anfragen außerhalb Bank (last 12mo) | 99 | Recency |
| 4 | Keine neuen Ratenkredite (last 12mo) | 66 | Recency |
| 5 | Immobilienkredit vorhanden | 55 | Asset |
| 6 | Bestätigte Identität | 38 | Verification |
| 7 | Ältester Bankvertrag (age) | 69 | Stability |
| 8 | Kein laufender Rahmenkredit | 36 | Debt |
| 9 | Aktuelle Adresse (age) | 94 | Stability |
| 10 | Keine Kreditkarte / Alter ältester Kreditkarte | 81 | Stability |
| 11 | Längste Restlaufzeit aller Ratenkredite | 61 | Debt |
| 12 | Laufender oder erledigter Ratenkredit (status) | 19 | History |
| | **TOTAL** | **999** | |

## Key Vocabulary

The portal mixes German banking terms that map to specific scoring behaviors. Agents must distinguish them:

| Term | English | Scoring relevance |
|---|---|---|
| Kreditkarte | Credit card (Visa/Mastercard, separate billing) | Factor #10 |
| Dispokredit / eingeräumte Kontoüberziehung | Overdraft on a checking account | Counted under the Girokonto contract (factor #7), **not** as a Kreditkarte |
| Rahmenkredit | Standalone revolving credit line | Factor #8 |
| Ratenkredit | Installment loan (consumer, auto, Bauspardarlehen) | Factors #11, #12; new ones hit factor #4 |
| Immobilienkredit / Kredit (grundpfandrechtlich gesichert) | Mortgage / mortgage-secured loan | Factor #5; **excluded** from factor #4 |
| Bonitätsprüfung Kreditkonditionen | Konditionsanfrage — rate-shopping inquiry | **Does not** affect score |
| Bonitätsanfrage / Kreditanfrage | Hard inquiry tied to an actual application | Counts toward factors #2 or #3 |
| Zahlungsstörung | Payment default (after at least 2 written reminders) | Catastrophic for factor #1 |
| Bonität | Creditworthiness | The whole score concept |
| Datensatznummer | Schufa record ID | Personal identifier on the data page |

## Module Inventory

| Module | URL anchor | `_skill.md` | Brief description |
|--------|---|:-----------:|-------------------|
| start | `/` | 30% | Dashboard / greeting, entry points to score and entries |
| score | `/bonidata/ngs` | 90% | 12-factor scoring methodology with full tier tables |
| entries | `/bonidata` | 70% | Vertragsdaten (contracts), Anfragen (inquiries), Zahlungsstörungen, Personendaten |
| identity-protection | `/identitaetsschutz` | 10% | Identity protection / data-leak monitoring (low coverage) |
| service | `/service` | 10% | Help, data correction, contact (low coverage) |

The `score/` module is the unique value of this pack: every other section is a standard data view, while the scoring methodology is published nowhere else in this level of detail. Agents recommending credit-improvement actions should treat `score/_skill.md` as the source of truth.

## Common Tasks for Agents

| Task | Approach |
|---|---|
| Look up the user's current score | Navigate `/bonidata`, read the "Mein Score" card |
| Diagnose why a score is below "Hervorragend" | Navigate `/bonidata/ngs`, read factor list, identify gaps below max |
| Estimate a credit-improvement action | Cross-reference the action against the tier tables in `score/_skill.md` |
| Verify no errors in the credit file | Navigate `/bonidata` → Vertragsdaten + Personendaten, audit against user's records |
| Check for payment defaults | Navigate `/bonidata` → Zahlungsstörungen — must be 0 |
| Confirm identity is verified | Navigate `/bonidata/ngs` → factor #6 "Bestätigte Identität" — must be +38 P |

## Gotchas

- **The "Mein Score" carousel may show "Ungenügend (kein Score)" as one slide** — this is generic explanatory copy about the score bands, not the user's actual score. The user's actual score is the labeled marker on the score card on `/bonidata` and the "Mein Score {N} von 999 Punkten" badge on `/bonidata/ngs`.
- **Score changes are lagged.** Tier crossings (e.g., address turning 4 years old) only register at the next quarterly recalc. A user who "just crossed" a tier will see their old tier until the recalculation date.
- **Konditionsanfragen vs Kreditanfragen.** Multiple rate inquiries on the same day are normal and harmless if they are flagged as "Bonitätsprüfung Kreditkonditionen". A bare "Bonitätsprüfung" or "Kreditanfrage" can hurt the score.
- **Mortgages are not Ratenkredite.** Despite both being long-term debt, Schufa treats them as separate factor inputs. Mortgages are excluded from the "new Ratenkredit" factor #4 and reward credibility via factor #5.
- **The data copy ("Datenkopie nach Art. 15 DSGVO")** is a separate, free product from `meineschufa.de`. The paid `app.schufa.de` subscription is what powers this portal. Casual visitors are funneled toward the paid product; pointing them at the data copy is a common consumer-rights tip.
