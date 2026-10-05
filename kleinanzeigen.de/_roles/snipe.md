---
name: Marketplace Sniping
match:
  - kleinanzeigen.de
---

# Marketplace Sniping — Kleinanzeigen

This role covers how an agent hunts a specific part on Kleinanzeigen when supply is scarce
and correctly-priced listings disappear within hours. It is a monitoring role, not a browsing
role: the work is detecting a new listing fast, deciding against a pre-agreed price band
without deliberation, and refusing everything that fails a mechanical check.

The role exists because scarce parts move faster than manual browsing. Well-priced listings
in a squeezed category are frequently claimed within a day of posting, so the win comes from
detecting a new ad early and deciding against a pre-agreed rule set — not from deliberating
once it is found.

Verify availability from the detail page before acting, via `kleinanzeigen_vet_listing`. Do
not hand-roll that check: the status labels are always present in the page markup and only
toggled by CSS, so a naive read marks live ads as sold.

## Preconditions

Do not start a hunt without these fixed in advance. Deciding them while a listing is live is
how buyers overpay for broken parts.

- **Target spec** — exact part, acceptable variants, and the disqualifying variants
- **Price band** — the observed used range, plus the snipe threshold (the number below which
  the answer is yes without further thought) and the walk-away ceiling
- **New-part reference price** — the hunt is pointless if used prices approach retail
- **Payment stance** — escrow-only, or escrow-plus-local-pickup
- **Deadline and fallback** — what gets bought new if nothing lands in time

## The Loop

Each cycle is search → diff → vet → decide. Cycles must be cheap enough to run on a schedule;
anything requiring judgement belongs in the preconditions, not the loop.

1. Run `kleinanzeigen_search` with the target keyword, `max_price` at the walk-away ceiling,
   and `exclude` set to the known-variant traps.
2. Diff the returned `ad_id` values against the ads already seen. Only new ids are work.
3. Run `kleinanzeigen_vet_listing` on each new id's URL, passing the same disqualifiers as
   `reject_pattern` and setting `require_identifier` when the category has model numbers.
   This step is mandatory — the search row cannot tell you whether the ad is still alive.
4. Apply the decision rules below. Escalate to the human only for a `CLEAR` verdict inside
   the snipe threshold, or for a judgement call the rules do not cover.
5. Persist the seen-id set so the next cycle does not re-report the same listings.

Run the loop on a schedule rather than continuously. Every 15–30 minutes is enough to catch
same-day listings; polling harder mostly re-reads unchanged pages.

## Decision Rules

Mechanical rules, applied to the `flags` array from `kleinanzeigen_vet_listing`. The point of
mechanising them is that a good-looking price should not be able to talk anyone out of a check.

| Verdict | Rule |
|---|---|
| **Drop silently** | `NOT_AVAILABLE` — reserved or deleted; nothing to do |
| **Drop silently** | `REJECT_PATTERN_MATCH` — hit a disqualifier you defined for this target |
| **Reject** | `CONDITION_DEFEKT` — the seller has declared it broken, whatever the price implies |
| **Reject** | Price far below the observed band with no explanation — that is bait, not luck |
| **Escalate with warning** | `FRESH_ACCOUNT`, or `NO_ESCROW` on a shipped (non-pickup) ad |
| **Escalate now** | No flags, price at or under the snipe threshold |
| **Note only** | `HIGH_CONTENTION` — informational; it means decide faster, not differently |

`NO_IDENTIFIER` is not a rejection on its own. It means identity is unconfirmed, so ask the
seller for the model or serial before paying, or treat a pickup-and-test as mandatory. It
only fires when the hunt declares that its category has identifiers at all.

## Buying Safely

- **"Direkt kaufen" is the only in-platform protection.** It is escrow with buyer protection.
  Everything else is a private handshake.
- **Never pay by bank transfer or PayPal Friends & Family.** A seller steering payment off
  the platform is the single most reliable fraud signal on this marketplace.
- **Prefer paying the asking price through escrow over negotiating.** In a fast market,
  haggling loses the item to a buyer who simply paid. Negotiate only on stale listings.
- **Private sellers exclude warranty** (`Gewährleistungsausschluss`) — legally normal here,
  but it means a dead part is the buyer's loss. Commercial sellers (`Gewerblicher Nutzer`)
  owe statutory warranty and issue invoices; that difference is worth a price premium on
  expensive or fragile items.
- **Test inside the protection window.** For memory, run an overnight memtest pass before the
  escrow window closes. For storage, read SMART data on arrival.
- **Ask for a serial or part-number photo** before paying on high-value items.

## Reporting

Report what a decision needs, not the full listing dump. Every escalation should carry the
price against the band, the verdict and flags, the availability status with its timestamp,
seller type and account age, whether escrow is on offer, and the direct URL. State plainly
when a hunt produced nothing — a quiet cycle is a valid, informative result, and padding it
with near-misses trains the reader to ignore the reports.

## Gotchas

- **Search results lie about availability** — the detail page is the only source of truth.
- **Cheapest in band is a warning, not a win** — check `Zustand` and the description before
  the price registers as good news.
- **Titles omit the disqualifying detail** — the variant that rules an item out (wrong
  format, wrong fitment, incomplete set) often appears only in the description or the model
  number, which is why `reject_pattern` runs against both.
- **A saved search (`Suchauftrag`) with push notifications is the human-side complement** to
  this loop; the agent covers structured vetting, the notification covers latency.
- **Do not automate the purchase itself.** Escalate to a human for payment. Sniping is about
  removing latency from detection and vetting, not about spending money unattended.
