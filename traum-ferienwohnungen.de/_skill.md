---
name: Traum-Ferienwohnungen Site
domain: traum-ferienwohnungen.de
match:
  - traum-ferienwohnungen.de
  - www.traum-ferienwohnungen.de
tags: ["@vacation-rental", "@germany", "@oyo", "@travel"]
learned: "2026-05-04"
last_verified: "2026-05-04"
confidence: 0.10
---

# Traum-Ferienwohnungen Site

German vacation-rental marketplace operated by Oyo (`traum-ferienwohnungen.de`). Homepage advertises 141,547 properties and "25+ Jahre Vertrauenslegacy". This pack is a **stub** — the session only loaded the homepage and a 404 page; no listing-search workflow was successfully completed. Confidence is low and the URL routing model is unverified.

## Browser Access

No login required for the homepage. A cookie consent dialog ("Cookie-Einstellungen") appears with options "Alle Cookies zulassen" and "Cookie Einstellungen". Search appears to require interacting with a JavaScript-driven autocomplete in the destination field (`input[placeholder="Wohin"]`).

## Known Surfaces

| URL | Result | Notes |
|---|---|---|
| `/` | Homepage with hero search box | Loads "Durchatmen" branding only after a 404 redirect from speculative URLs |
| `/suche/?location=Brandenburg&arrival=YYYY-MM-DD&departure=YYYY-MM-DD&persons=N` | Loads but ignores location filter — shows global 141,500 result count | Param names suspected wrong |
| `/ferienhaus/brandenburg/` | 404 | |
| `/brandenburg-region/` | 404 | |
| `/ferienhaus/brandenburg/uebersicht/?...wassernaehe=direkt` | 404 | Speculative URL did not resolve |

The actual production URL pattern for filtered searches is **unknown**. The homepage search form likely posts to a discovered destination ID rather than honoring `?location=` in the URL.

## Homepage Search Form

| Element | Selector | Notes |
|---|---|---|
| Destination input | `input[placeholder="Wohin"]` | Triggers autocomplete dropdown — must select an entry rather than just typing |
| Anreise / Abreise | Calendar widget | Not exercised |
| Gäste | Selector with default "2 Gäste" | |
| Suchen button | "Suchen" with magnifier icon | Submits |

`type` into the destination input did not produce visible autocomplete in this session — interaction model needs further investigation (possibly requires focus + key events).

## Known Domain Knowledge

- Top-level navigation: Urlaubsideen, Angebote, Hilfe, Wunschliste, Anmelden, Vermieten
- Country tabs: Deutschland, Österreich, Dänemark, Niederlande, Italien, Kroatien, Spanien, Frankreich, Schweiz
- Two booking modes advertised on homepage: "Anfrage Stellen" (request to host) and "Aufenthalt Direkt Buchen" (instant book)
- Tagline: "Ferienwohnungen & Ferienhäuser direkt von privat mieten" — direct private-host marketplace
- 404 page is themed ("Durchatmen" with cloud illustration) — not a server error indicator on its own; the site likely catches all unknown URLs into this fallback

## Unknown / Not Tested

Almost everything:
- Working URL pattern for filtered search
- Listing card / detail page selectors
- Filter set (waterfront, garden, persons, dates)
- Login, favorites, message hosts
- Booking flow ("Anfrage" vs "Direkt Buchen")
- API endpoints
- Mobile site
- Pricing model and per-night vs per-stay display
- Cancellation, deposits, host policies
- Pagination and sort
- Map view

## Suggested Next Steps for Pack Maturation

1. Click into the homepage destination autocomplete with `mouse_move + click` (or `focus + ArrowDown + Enter`), select a specific region, and capture the resulting URL.
2. Once on a results page, capture pagination, sort, and filter URL parameters.
3. Open one detail page and document the field layout.
4. Confirm whether listings expose JSON-LD structured data (common for German vacation-rental sites).
