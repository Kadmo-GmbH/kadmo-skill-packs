---
name: Fewo-direkt Vacation Rentals
domain: fewo-direkt.de
match:
  - fewo-direkt.de
  - www.fewo-direkt.de
tags: ["@vacation-rental", "@vrbo", "@expedia", "@german", "@travel"]
learned: "2026-05-04"
last_verified: "2026-05-04"
confidence: 0.30
---

# Fewo-direkt Vacation Rentals

German-language Vrbo/Expedia property (`fewo-direkt.de`). Sister sites listed in the footer: Vrbo (com), Abritel.fr, Bookabach.co.nz, Stayz.com.au — all share the same Expedia codebase. This pack reflects a single shallow exploration session focused on Brandenburg vacation rentals — search results page and one detail page were inspected; large parts of the platform were not exercised.

## Browser Access

No login required for browsing search results or opening listing detail pages. A consent dialog (Cookie banner with "Akzeptieren / Alle ablehnen") shows on first visit. Detail pages render heavy lazy-loaded React — only above-the-fold content is in initial DOM.

## Known Surface Map

| URL pattern | Purpose | Confidence |
|---|---|---|
| `/` | Homepage with destination/date/guest search | medium |
| `/search?destination=...&startDate=...&endDate=...&adults=N` | Search results page | high |
| `/ferienwohnung-ferienhaus/p<ID>` | Individual property detail | high |
| `/Vrbo-Liste/<region>.dt-<id>` | Speculative — observed in Hilfe links, returns 404 with my params | low |
| `/lp/...` / `/helpcenter` / `/p/home` | Marketing / help / partner | none |

## Confirmed URL Filter Parameters

| Parameter | Effect | Notes |
|---|---|---|
| `destination=<text>` | Free-text destination | URL-encoded with `+` for spaces |
| `regionId=<numeric>` | Internal region ID — auto-attached after search | Brandenburg = `6049339` |
| `startDate=YYYY-MM-DD` / `endDate=YYYY-MM-DD` | Date window | |
| `adults=N` | Guest count (children count not yet tested) | |
| `sort=RECOMMENDED` | Default sort | Other values not enumerated |

**Unknown:** waterfront/amenity URL params (UI filter button observed but did not visibly map to a URL parameter), bedroom count param, price range, property type, distance-to-water filter.

## Listing Card — Known Selectors

The result list does not expose stable test IDs in observation; effective extraction uses indirect signals:

| Element | How to find | Notes |
|---|---|---|
| Listing detail link | `a[href*="/ferienwohnung-ferienhaus/p"]` | Each card wraps a link with id pattern `p<digits>` |
| Listing ID | regex `p(\d+)` from href | Numeric |
| Inline price | `top_dp=<n>` query param embedded in card link | Stays/week total in EUR |
| Card text block | walk up from anchor to ancestor containing "Schlafzimmer" | Innerhalb that card find title, location, beds |
| Title block | line above "Schlafzimmer · N Schlafzimmer · M Betten" in innerText split | Heuristic only |

Scraping pattern that worked in session: collect distinct `/ferienwohnung-ferienhaus/p<id>` hrefs, parse `top_dp` for price, walk DOM upward to read surrounding text block for title/distance/beds.

## Listing Detail Page — Known Fields

Initial DOM size is small (~1.3 KB innerText) — most content lazy-loads on scroll. Above-the-fold:

| Field | How to find | Notes |
|---|---|---|
| Title | `h1` | Property title incl. region descriptor |
| Capacity | innerText "Platz für N Gäste" | German |
| Bedrooms / bathrooms / m² | innerText "N Schlafzimmer" / "M Badezimmer" / "Y m²" | German |
| Rating | innerText "9,X von 10" + count "M Bewertungen" | German number format (comma) |
| Highlights | "Highlights" section | E.g. "Tischtennis vor Ort" |
| Amenities | "Beliebte Annehmlichkeiten" + "Alle anzeigen" | German labels |
| Distance markers | "X Gehminuten" / "Y Autominuten" | Travel times to nearby places |

To get the full description, sleeping arrangement, host info, and reviews — must scroll/wait for lazy content; no working pattern captured in session.

## Filter UI

A "Filter" button is present on the search results header. Clicking it visibly opened nothing in observation — the modal may render in an off-screen panel or require a second interaction. The filter for waterfront is **suspected to exist** but was not located via the UI; URL parameter `filterByAmenity=WATERFRONT` was tried and returned 404 with the wrong path. Concrete waterfront filter mechanism remains **unknown**.

## Known Domain Knowledge

- Pricing displayed as a 7-night total (e.g. "836 € für 1 Ferienunterkunft, 7 Nächte"), with per-night breakdown ("119 € pro Nacht"). Some listings include taxes & fees in the headline price; others surface them at checkout.
- "Premium-Gastgeber" badge marks certain hosts (Vrbo super-host equivalent).
- Region tabs in side panel: Potsdam, Cottbus, Brandenburg (city), Oranienburg, Eberswalde, Neuruppin, Bad Saarow, Rheinsberg, Werder (Havel) — each with its own `regionId`.

## Known Gotchas

- **Detail pages render only ~1.3 KB above-the-fold**. Reading `body.innerText` right after navigation yields title, capacity, headline amenities, and distances only. Description, sleeping arrangement, host info, and reviews require scroll-and-wait.
- **Filter button does not open a modal in a single click in observation** — interaction model unclear.
- **Some construced URLs 404**. The `/Vrbo-Liste/...?petIncluded=...&minBedrooms=...` pattern returned a not-found page; the working search URL is `/search?...`.
- **Cookie consent banner blocks first interaction**. Click "Akzeptieren" or "Alle ablehnen" before any other actions.
- **Price displayed in card link's `top_dp` query param** is a useful shortcut for bulk extraction without parsing visible card text.

## Unknown / Not Tested

- Login, account, booking, messaging, payment
- Filter URL parameter encoding (waterfront, bedrooms, price)
- Map-based search interactions
- Property-type filter, accessibility, kid-friendly, pet-friendly URL params
- Sort orders beyond `RECOMMENDED`
- Description and review extraction from detail pages (requires lazy-load handling)
- Mobile site DOM
- Cross-region pagination
- Internal region IDs catalog (only Brandenburg=6049339 noted)
