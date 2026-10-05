---
name: Kleinanzeigen Classifieds
domain: kleinanzeigen.de
match:
  - kleinanzeigen.de
  - www.kleinanzeigen.de
tags: ["@classifieds", "@germany", "@marketplace", "@rentals", "@private-sellers", "@sniping"]
learned: "2026-05-04"
last_verified: "2026-08-17"
confidence: 0.75
---

# Kleinanzeigen Classifieds

German general-purpose classifieds (formerly eBay Kleinanzeigen), heavy private-seller bias.
Two flows are mapped: **general keyword search** (any category — verified on PC hardware,
2026-08-17) and **Wohnen auf Zeit / WG rentals in Berlin** (2026-05-04). The generic
search + detail surfaces below apply to every category; category-specific detail keys do not.

## Browser Access

No login required for browsing, searching, or extracting listing data. A "Hallo!" welcome
popup appears on first visit. Posting, messaging, favourites, and saved searches
(*Suchaufträge*) require an account.

## Known Surface Map

| URL pattern | Purpose | Confidence |
|---|---|---|
| `/s-<keyword-slug>/k0` | **Global keyword search, all categories** | high |
| `/s-sortierung:neueste/<keyword-slug>/k0` | **Same, newest-first** — the sniping entry point | high |
| `/s-seite:<N>/<keyword-slug>/k0` | Result page N — **25 cards per page regardless of total** | medium |
| `/s-anzeige/<title-slug>/<adId>-<categoryId>-<locationId>` | Individual listing detail | high |
| `/stadt/<city>/` | City landing page with categories | medium |
| `/s-<category>/<city>/c<categoryId>l<locationId>` | Category × city results | high |
| `/s-<category-pair>/<city>/<keyword>/k0c<categoryId>l<locationId>` | Category × city × keyword | high |

Multi-word keywords are hyphen-joined in the slug (`64gb-ddr5-2x32`). The trailing `k0`
means "no category filter". Sort tokens sit **before** the keyword segment, not as a query
parameter.

Category / location IDs (rentals flow only):

| Slug | ID |
|---|---|
| `s-wohnen-auf-zeit` / `s-auf-zeit-wg` | category 199 |
| Berlin | location 3331 |

## Results Page — Card Extraction

> ⚠️ **The pre-2026-08 class selectors are dead.** The site migrated to utility CSS
> (Tailwind-style). `article.aditem`, `.aditem-main--*`, `.aditem-main--middle--price-shipping--price`
> and siblings now match **zero** elements. Anything written against them fails silently
> with empty results rather than an error.

Stable hooks after the migration:

| Element | Selector / source | Notes |
|---|---|---|
| **Card container** | `article[data-adid]` | The reliable card root |
| **Ad ID** | `data-adid` attribute | Stable primary key — use for dedupe across runs |
| **Canonical path** | `data-href` attribute | Relative `/s-anzeige/...`; prefix the origin |
| Title | `a` inside the card, or JSON-LD `title` | |
| Price | leaf `<p>` matching `€` | Formats: `1.000 €`, `800 € VB`, `Zu verschenken`, `VB`, or absent |
| Posted at | leaf `<span>` | `Heute, 21:44` / `Gestern, 09:12` / `15.08.2026` — **time-of-day only on today/yesterday** |
| Location | leaf `<span>` | `<PLZ> <Ort>`, e.g. `10115 Berlin` |
| Badges | `<span>` text match | `Direkt kaufen` (escrow available), `Versand möglich` (ships) |
| Per-card JSON-LD | `script[type="application/ld+json"]` in the card | See correction below |

**JSON-LD correction (2026-08-17):** the per-card payload is an **ImageObject**, carrying
only `creditText`, `title`, `description`, `contentUrl`, `representativeOfPage`, `@context`,
`@type`. It does **not** carry `offers.price` / `offers.priceCurrency`, contrary to the
2026-05 note in this file. Price must come from the card DOM or the detail page.

## Listing Detail Page — Known Fields

| Field | Selector | Notes |
|---|---|---|
| Title | `#viewad-title` | Carries the status prefix — see states below |
| Price | `#viewad-price` | `949 € VB`, `1.600 €` |
| Detail key/values | `#viewad-details` (rows `li.addetailslist--detail`) | Includes `Art` and `Zustand` |
| Post date + view count | `#viewad-extra-info` | e.g. `16.07.2026 280` — date then **view counter** |
| Description | `#viewad-description-text` | Free-form German; where part numbers live |
| Seller block | `#viewad-contact` | Name, badges, user type, tenure, ad count |
| Location | `#viewad-locality` | `<PLZ> <Bundesland> - <Ort>`, e.g. `10115 Berlin - Mitte` |
| Escrow offer | text `Direkt kaufen` present on page | Buyer-protection purchase available |

`Zustand` (condition) enum: `Neu`, `Sehr Gut`, `Gut`, `In Ordnung`, `Defekt`.

Seller block fields: display name · satisfaction badge (`TOP Zufriedenheit`, `SUPER TOP`,
`OK`) · friendliness / reliability badges · `Privater Nutzer` vs `Gewerblicher Nutzer` ·
`Aktiv seit <DD.MM.YYYY>` (account age) · `N Anzeigen online`.

## Listing States

> ⚠️ **The status labels are always in the DOM.** `#viewad-title` always contains two
> `span.pvap-reserved-title` elements — `Reserviert •` and `Gelöscht •` — and the site
> toggles them with an `is-hidden` class (`display: none`) instead of adding or removing
> them. So `textContent` on the title reports **every listing as deleted**, including
> brand-new live ones. This is a silent, total false positive: nothing errors, every ad
> just looks sold.

Read status from rendered elements only:

| State | How to detect | Meaning |
|---|---|---|
| Live | no `.pvap-reserved-title` renders (all carry `is-hidden`) | Available |
| Reserved | a rendered span containing `Reserviert` | Committed to another buyer |
| Deleted | a rendered span containing `Gelöscht` | Sold or withdrawn |

Use `innerText` (which honours CSS visibility) for the clean title, or filter
`.pvap-reserved-title` by computed `display`. Never use `textContent` for either.

Reserved/deleted ads were **not** observed in search results during the 2026-08-17 sweep —
across four keyword searches, zero result cards carried a reserved or deleted marker, which
suggests closed ads are dropped from results rather than lingering. Detail-page status
remains the authoritative check before acting, but treating every result row as presumed-dead
is not supported by evidence.

**Verification status:** the live path is confirmed against real listings. The
reserved/deleted path is confirmed by mechanism — the `is-hidden` toggle and the two
inline-styled label spans are unambiguous — but has **not** yet been observed on a listing
in that state, because no reserved ad was found in the wild to test against.

## Known Domain Knowledge

- **Velocity is measurable.** `#viewad-extra-info` exposes a view counter alongside the post
  date. Well-priced hardware listings were reserved at 25–100 views, often the same day.
  View count ÷ age is a usable "how contested is this" proxy.
- **`Direkt kaufen` is the only in-platform protection.** It is Kleinanzeigen's escrow
  purchase (Käuferschutz). Everything else is a private handshake — `Privatverkauf` ads
  carry an explicit `Gewährleistungsausschluss` (warranty exclusion), so a bad part is the
  buyer's loss unless bought through escrow or collected and tested in person.
- **Private vs commercial matters legally.** `Gewerblicher Nutzer` sellers owe statutory
  warranty and issue invoices; `Privater Nutzer` sellers exclude it.
- **Condition is a structured field, not just prose.** `Zustand: Defekt` appears in
  `#viewad-details` even when the title and price look like a bargain — the cheapest listing
  in a price band is frequently the broken one.
- Ad IDs and the `/s-anzeige/<slug>/<id>-<cat>-<loc>` shape are stable across sessions.
- Sort tokens compose with keyword search; `sortierung:neueste` is the only one verified.

## Category Notes — PC Hardware / RAM (verified 2026-08-17)

- **Laptop memory is routinely listed as if it were desktop memory.** SO-DIMM kits surface
  under desktop searches with no visual cue in the title. Discriminate on part-number suffix
  (Crucial `…U5` = desktop UDIMM vs `…S5` = SO-DIMM), on the words `SO-DIMM`/`SODIMM`, or on
  description tells like "aus einem Notebook ausgebaut". Several listings priced ~40% under
  the desktop band were SO-DIMM.
- Descriptions usually contain the exact manufacturer part number
  (`KF560C30BBEK2-64`, `CMK96GX5M2B6000Z30`, `CT2K32G48C40U5`) — the highest-signal field for
  identity, more reliable than the title.
- Kit topology (`2x32` vs `4x32` vs `2x48`) drives value and is often only in the description.

## Known Gotchas

- **Welcome popup** (`Hallo!`) blocks first interaction — press Escape or click X.
- **Legacy `.aditem*` selectors silently return nothing** post-migration (see above).
- **Search-results status is stale** — always re-open the detail page before acting.
- Keyword in a category URL path (`/berlin/wasser/c199l3331`) does **not** filter; the
  working shape comes from form submission (`/s-auf-zeit-wg/berlin/<keyword>/k0c199l3331`).
- `type` + Enter into the search input did not navigate in one test; `form.submit()` worked.
- Price strings need normalising: thousands separator is `.`, `VB` (*Verhandlungsbasis*)
  means negotiable, and `Zu verschenken` / empty means no price.
- Automated fetchers are blocked at the HTTP layer (403) — these surfaces require a real
  browser session, not a plain fetch.

## Unknown / Not Tested

- Login, posting, messaging, favouriting, and **creating Suchaufträge programmatically**
- The `Direkt kaufen` checkout flow itself (only its presence is detected)
- Pagination beyond page 1; sort tokens other than `sortierung:neueste`
- Sidebar filter URL encoding (price/condition/shipping facets)
- Radius / PLZ search parameters
- Seller-profile pages and rating history
- Image gallery / video extraction; mobile site; native API endpoints
- City IDs other than Berlin (3331)
