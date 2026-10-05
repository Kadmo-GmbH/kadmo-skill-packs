---
name: Score Methodology
domain: schufa.de
path: /bonidata/ngs
parent: schufa.de
tags: ["@scoring", "@methodology", "@credit-score", "@bonität", "@2026-reform"]
learned: "2026-05-11"
last_verified: "2026-05-11"
confidence: 0.95
---

# Score Methodology

This module documents the **2026 Schufa scoring methodology** in full, including every factor's complete tier table as published in `/bonidata/ngs/*` detail pages. It is the unique value of the pack — agents recommending credit-improvement actions should treat this file as the source of truth.

The score is a single integer between **100 and 999**. It is the simple sum of 12 factor scores, each capped at a published maximum. The system replaced Schufa's previous proprietary scoring on **17 March 2026**.

## Page Structure

The `/bonidata/ngs` page is the user's personal score view. It contains:

```
+-- "Mein Score" badge — single number out of 999 -----+
+-- Band label: "Hervorragend / Gut / Akzeptabel / ..." +
+-- Carousel of explanatory cards (5 band descriptions) +
+-- "Score-Berechnung:" section ------------------------+
|   12 rows, each: [factor name] [+N P] [arrow]         |
|   Final row: "Ihr Score-Wert  =  {N} P"               |
+-- "Der neue SCHUFA-Score" tutorial card ---------------+
+-- "Feedback" / Umfrage card --------------------------+
```

Each factor row navigates to a detail page at `/bonidata/ngs/{factorKey}` showing:

- The factor's name + the user's point award
- A prose explanation of the criterion
- The **complete tier table** with all possible point values
- For some factors: the qualifying contract or data point (e.g., the address date that triggered the tier)

## Score Bands

The detail card displays a vertical bar chart with five bands:

| Label | Range | Notes |
|---|---|---|
| Hervorragend (Excellent) | 776–999 | Top band, dark green |
| Gut (Good) | 709–775 | Green, ~20% of population |
| Akzeptabel (Acceptable) | 642–708 | Yellow |
| Ausreichend (Sufficient) | 100–641 | Orange |
| Ungenügend (kein Score) | — | Red; insufficient data, no score assignable |

The user's score is drawn as a labeled marker on the appropriate band.

## The 12 Factors

The score is computed as `score = Σ factor_i.points`. Each factor is independent; there are no multiplicative interactions or weights beyond the per-factor maxima below.

| # | Factor key (URL suffix) | German label | Max P |
|---|---|---|---:|
| 1 | `paymentDefaults` | Keine Zahlungsstörung | 264 |
| 2 | `bankInquiries12m` | Keine neuen Konto- & Kreditkartenanfragen oder Abschlüsse | 117 |
| 3 | `nonBankInquiries12m` | Weitere Anfragen | 99 |
| 4 | `newInstallmentLoans12m` | Keine neuen Ratenkredite | 66 |
| 5 | `mortgage` | Immobilienkredit | 55 |
| 6 | `identityVerified` | Bestätigte Identität | 38 |
| 7 | `oldestBankContractAge` | Ältester Bankvertrag | 69 |
| 8 | `currentRahmenkreditAge` | Kein laufender Rahmenkredit | 36 |
| 9 | `currentAddressAge` | Aktuelle Adresse | 94 |
| 10 | `oldestCreditCardAge` | Keine Kreditkarte | 81 |
| 11 | `longestInstallmentRemaining` | Ratenkredite mit Restlaufzeit | 61 |
| 12 | `installmentLoanStatus` | Laufender oder erledigter Ratenkredit | 19 |
| | | **TOTAL** | **999** |

Factor key names are observed/normalized — actual URL paths may differ; the address detail page is confirmed at `/bonidata/ngs/currentAddressAge`.

---

## Factor #1 — Keine Zahlungsstörung (max 264)

The single heaviest factor. Rewards a clean payment-default history.

**Criterion:** "Alter erledigter Zahlungsstörungen / Inkasso-Anfragen" — age of resolved payment defaults / collection inquiries.

A Zahlungsstörung is reported when a payable amount is not paid, not contested, and remains unpaid after at least **two written reminders (Mahnungen)**, AND the consumer was warned about Schufa reporting at the first Mahnung.

| Status | Score |
|---|---:|
| keine Zahlungsstörung | **+264 P (max)** |
| ab 2 Jahren erledigt | +152 P |
| ab 1 Jahr erledigt | +135 P |
| Zahlungsstörung erledigt (kürzlich) | +100 P |
| offen / nicht erledigt | implied 0 P (catastrophic) |

### Improvement levers

- **Prevent:** Auto-debit (Lastschrift) for every recurring bill — utilities, telecom, insurance, rent, BNPL. A single missed payment after two unanswered Mahnungen wipes out 264 points.
- **Recover:** Pay any outstanding Forderung within **100 days** under the 2026 reform — paid defaults are deleted significantly faster than the old 3-year rule.
- **Older defaults still hurt.** Even "resolved 2+ years ago" caps you at 152 P (-112).

---

## Factor #2 — Keine neuen Konto- & Kreditkartenanfragen (max 117)

Counts applications and openings of new Girokonten and Kreditkarten in the **last 12 months**.

**Criterion:** "Anzahl Anfragen und Abschlüsse für Girokonten und Kreditkarten in den vergangenen 12 Monaten."

Only "Bonitätsanfragen" tied to actual applications and account openings count. Pure **Konditionsanfragen** ("Bonitätsprüfung Kreditkonditionen" — rate-shopping inquiries) do **not** count here.

| Count | Score |
|---|---:|
| keine Anfrage / Abschluss | **+117 P (max)** |
| 1 Anfrage / Abschluss | +82 P |
| 2 Anfragen / Abschlüsse | +44 P |
| ab 3 Anfragen / Abschlüsse | +0 P |

### Improvement levers

- **Wait it out.** Old inquiries roll off after 12 months automatically.
- **Use Konditionsanfrage**, not Kreditanfrage, when shopping rates — most banks will accept this if asked. Konditionsanfragen are invisible to the score.
- **Consolidate applications.** If you must apply, open one product at one bank rather than two.

---

## Factor #3 — Weitere Anfragen außerhalb des Bankenbereichs (max 99)

Like factor #2 but for non-bank inquiries: online retail (rechnung/raten purchases), telecom contracts, insurance, mobility, etc.

**Criterion:** "Anzahl Anfragen außerhalb des Bankenbereichs in den vergangenen 12 Monaten."

| Count | Score |
|---|---:|
| keine oder 1 Anfrage | **+99 P (max)** |
| 2 Anfragen | +71 P |
| 3 Anfragen | +40 P |
| mehr als 3 Anfragen | +0 P |

### Improvement levers

- **Limit BNPL / Klarna / Rechnungskauf.** Each high-risk online checkout that triggers a Bonitätsprüfung adds an inquiry.
- **Don't switch mobile/insurance contracts opportunistically** if you're applying for a major loan in the next year.

---

## Factor #4 — Keine neuen Ratenkredite (max 66)

Counts **new installment loans** taken in the **last 12 months**. Consumer credit, auto loans, electronics financing, furniture installment plans.

**Excluded:** Immobilienkredite (mortgages) — they are scored separately under factor #5.

| Count | Score |
|---|---:|
| kein Kredit | **+66 P (max)** |
| 1 Kredit | +48 P |
| 2 Kredite | +32 P |
| ab 3 Kredite | +0 P |

### Improvement levers

- **Avoid layering loans.** Statistically, multiple new loans correlate with default risk.
- **A new Ratenkredit immediately costs at minimum 18 points here**, on top of any inquiry-side hit on factor #2.

---

## Factor #5 — Immobilienkredit (max 55, binary)

Rewards holding (or having guaranteed) a mortgage. Statistically, mortgage holders are lower default risk because banks pre-screened them heavily.

| Status | Score |
|---|---:|
| nicht vorhanden | +0 P |
| vorhanden | **+55 P (max)** |

This factor is binary — there is no partial credit. Once any mortgage is on file, the user gets the full 55 points.

---

## Factor #6 — Bestätigte Identität (max 38, binary)

Rewards a verified identity. Achieved automatically when a German bank verifies the user's Personalausweis at account opening (PostIdent / VideoIdent). Required by law for German bank contracts.

| Status | Score |
|---|---:|
| nicht überprüft | +0 P |
| überprüft | **+38 P (max)** |

Once any one bank has verified the user, the flag is permanent. Most German residents with a Girokonto have this.

---

## Factor #7 — Ältester Bankvertrag (max 69)

Age of the user's oldest active "Bankvertrag" on file. Schufa defines Bankverträge to include: Girokonten, Kreditkarten, Leasingverträge, Bürgschaften, Bauspardarlehen, and Kredite.

| Age | Score |
|---|---:|
| kein Bankvertrag | +18 P |
| bis zu 3 Monate | +0 P |
| ab 3 Monate | +3 P |
| ab 6 Monate | +6 P |
| ab 1 Jahr | +12 P |
| ab 2 Jahren | +17 P |
| ab 3 Jahren | +23 P |
| ab 4 Jahren | +27 P |
| ab 5 Jahren | +37 P |
| ab 10 Jahren | +49 P |
| ab 15 Jahren | +56 P |
| ab 20 Jahren | **+69 P (max)** |

### Counter-intuitive details

- **"kein Bankvertrag" (+18 P) scores higher than "0–3 Monate" (+0 P).** The first 3 months of any new bank relationship statistically correlate with elevated default risk, so opening a brand-new account temporarily lowers the score.
- **Never close your oldest bank contract.** Closing it forfeits accrued tier progress and may drop the score by 10–50 points depending on what's left.
- **If you have a younger backup account**, closing the oldest defers the next tier crossing by the age gap. Closing has near-zero immediate impact if the next-oldest contract is in the same tier.

---

## Factor #8 — Kein laufender Rahmenkredit (max 36)

A Rahmenkredit is a flexible standalone revolving credit line (similar to a Dispokredit but as a separate product with better terms and higher limits). This factor penalizes new Rahmenkredite and rewards either no Rahmenkredit or an older one.

**Critical distinction:** A Dispokredit (overdraft attached to a Girokonto) is **NOT** a Rahmenkredit. Dispokredite are part of the Girokonto contract.

| Age of youngest Rahmenkredit | Score |
|---|---:|
| kein Rahmenkredit | **+36 P (max)** |
| bis zu 1 Jahr | +0 P |
| ab 1 Jahr | +16 P |
| ab 2 Jahren | **+36 P (max)** |

Opening a Rahmenkredit costs 36 points for the first year and recovers to max at year 2.

---

## Factor #9 — Aktuelle Adresse (max 94)

Time at the user's current registered address. Statistically, residential stability is one of the strongest signals of payment reliability.

| Age | Score |
|---|---:|
| bis zu 7 Monate | +0 P |
| ab 7 Monate | +6 P |
| ab 1 Jahr | +17 P |
| ab 2 Jahren | +28 P |
| ab 3 Jahren | +35 P |
| ab 4 Jahren | +40 P |
| ab 5 Jahren | +42 P |
| ab 6 Jahren | +49 P |
| ab 7 Jahren | +49 P |
| ab 8 Jahren | +54 P |
| ab 9 Jahren | +58 P |
| ab 10 Jahren | +63 P |
| ab 15 Jahren | +75 P |
| ab 20 Jahren | **+94 P (max)** |

### Improvement levers

- **Stay put.** Every year at the same address adds points up to a maximum of 94 at 20 years.
- **Moving costs 0–94 points.** A move to a new address resets the counter to "bis zu 7 Monate" = 0 P. The pain dwarfs any other lever in the system.
- **Note the duplicate tier "ab 6 / ab 7 Jahren = +49 P".** Schufa's tier structure flattens at year 6–7; the next bump comes at year 8.

---

## Factor #10 — Keine Kreditkarte / Alter ältester Kreditkarte (max 81)

Age of the user's oldest Kreditkarte (real Visa/Mastercard/Amex with a separate monthly billing cycle). Counter-intuitive: **having NO credit card is better than having one for less than 1 year.**

| Status / age | Score |
|---|---:|
| keine Kreditkarte | +24 P |
| bis zu 6 Monate | +0 P |
| ab 6 Monate | +14 P |
| ab 1 Jahr | +23 P |
| ab 2 Jahren | +32 P |
| ab 3 Jahren | +41 P |
| ab 4 Jahren | +52 P |
| ab 5 Jahren | +59 P |
| ab 10 Jahren | +74 P |
| ab 15 Jahren | **+81 P (max)** |

### Counter-intuitive details

- **Opening a credit card initially LOWERS the score.** First 6 months: 0 P (-24 vs no card). First year: 14–23 P (still below "no card"). Breaks even around year 1, becomes net positive at year 2.
- **The reward compounds slowly.** Holding a card for 5 years gains +35 P over "no card"; holding for 15 years gains +57 P.
- **Closing your oldest card is destructive.** It resets the timer to whichever card is now oldest, and may drop the user to "keine Kreditkarte" if it was the only one.

### Improvement levers

- **Only open a credit card if you will hold it for 2+ years.** Anything less is a net loss.
- **Open via your existing primary bank** to minimize the factor #2 hit (some banks treat in-house upgrades as a softer inquiry).
- **Set up full-balance autopay** every month so the new card doesn't risk a Zahlungsstörung on factor #1.

---

## Factor #11 — Ratenkredite mit Restlaufzeit (max 61)

The factor name suggests it rewards "having installment loans with remaining term", but the **opposite is true**: this factor rewards **shorter** remaining terms because long-term obligations represent extended default exposure.

**Criterion:** "Längste Restlaufzeit aller Ratenkredite" — longest remaining term across all open installment loans.

| Longest remaining term | Score |
|---|---:|
| kein Ratenkredit / bis zu 3 Jahren | **+61 P (max)** |
| ab 3 Jahren | +48 P |
| ab 4 Jahren | +28 P |
| ab 5 Jahren | +10 P |
| ab 6 Jahren | +0 P |

### Counter-intuitive details

- **A long-term Bauspardarlehen reported as a Ratenkredit costs the full 61 points.** Bauspardarlehen with 200+ month rate plans typically fall into "ab 6 Jahren" until they are nearly paid off.
- **Mortgages are scored separately** (factor #5) and do not appear here.
- **Paying off an installment loan early can unlock the full 61 points in one quarterly recalculation.** Combined with factor #12, the swing can be **+80 P**.

---

## Factor #12 — Laufender oder erledigter Ratenkredit (max 19)

Rewards a positive credit history. The optimal state is "I once had an installment loan, paid it off in full, no longer have it."

| Status | Score |
|---|---:|
| offener oder negativ erledigter Ratenkredit | +0 P |
| kein Ratenkredit | +9 P |
| positiv erledigter Ratenkredit | **+19 P (max)** |

### Strategic implications

- **An open loan with no negative history still scores 0** — Schufa rewards the act of having successfully completed a loan, not the act of having one currently.
- **Combined with factor #11, paying off any open Ratenkredit is the highest-leverage single action** in the entire system: up to +80 P (61 + 19) in one move.
- **Taking a loan purely to "build credit history" is a bad trade.** A new loan hits factor #4 (-66 P minimum) and factor #11 (negative or 0 depending on term), with only +19 P of long-run upside on factor #12 after eventual payoff.

---

## How To Reason About Improvement

A useful diagnostic flow when an agent is asked "how can I improve my Schufa score?":

1. **Pull the 12 factor scores from `/bonidata/ngs`.**
2. **Sum the gap to max per factor.** Total gap = 999 − current.
3. **Classify each gap as `time-bound` or `action-bound`.**
   - Time-bound gaps (#7, #9, #10): only fixable by waiting. No action improves them this quarter.
   - Action-bound gaps (#11, #12): require a financial decision (loan payoff, account change).
   - Behaviour-bound gaps (#1, #2, #3, #4): protect by not doing the wrong thing.
4. **Estimate the next quarterly recalc impact** by checking whether any age-tier crossings have happened since the last "Ermittelt am" date.
5. **Recommend in order:** (a) preserve current state, (b) wait for free tier crossings, (c) consider high-leverage payoffs only if a near-term credit event justifies the cash outflow.

## Improvement-Lever Cheat Sheet

| Goal | Action | Cost | Reward | Time horizon |
|---|---|---|---|---|
| Hit Hervorragend from upper Gut | Do nothing — wait for next quarterly recalc if any age tier just crossed | €0 | +3 to +15 P | <3 months |
| Big one-shot boost | Pay off open Ratenkredit in full | Remaining balance | up to +80 P | next recalc |
| Long-term ceiling | Open one credit card, hold 5+ years | inquiry hit upfront | up to +35 P over 5y, +57 P over 15y | 2y to 15y |
| Preserve all gains | Don't move, don't close oldest contracts, use Konditionsanfragen | discipline | preserves -200+ P from getting worse | ongoing |
| Recover from a Zahlungsstörung | Pay within 100 days | the amount owed | restores factor #1 fast | ~100 days |

## What NOT To Do

| Anti-pattern | Why it fails |
|---|---|
| Open a credit card "to build credit" | First year is net negative on factor #10 |
| Take a new installment loan to improve factor #12 | Loses factor #4 immediately (-66 P) |
| Close unused old accounts to "tidy up" | Forfeits factor #7 progress; downgrades the band |
| Move to a "better" address | Resets factor #9 to 0 |
| Apply at multiple banks one after another | Multiplies factor #2 hits unless flagged as Konditionsanfrage |
| Ignore a Mahnung even briefly | A formal Zahlungsstörung wipes 264 P |
| Pay for "Schufa cleaning" services | Disputes can be filed for free directly with Schufa |
