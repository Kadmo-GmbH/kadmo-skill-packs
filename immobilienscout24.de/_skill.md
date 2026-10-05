---
name: ImmoScout24 Real Estate
domain: immobilienscout24.de
match:
  - immobilienscout24.de
  - www.immobilienscout24.de
tags: ["@real-estate", "@germany", "@rentals", "@wohnen-auf-zeit", "@property"]
learned: "2026-05-04"
last_verified: "2026-05-04"
confidence: 0.50
---

# ImmoScout24 Real Estate

Germany's dominant real-estate portal (`immobilienscout24.de`). The session focused on the **"Wohnen auf Zeit"** (furnished short-term/temporary living) category for Berlin and Brandenburg — that flow is well mapped. Other categories (Mieten/Kaufen/Bauen for Wohnung/Haus/Grundstück, Gewerbe, Garage) share the same shell but use different routes and filter sets and are **not** mapped here in detail.

## Browser Access

Logged-in state was used in the session (user was signed in as M. S.), but no behaviour seemed to require it for read access. Saved searches ("Suche speichern"), favoriting ("Merkzettel"), and posting an inserate require login.

## Known Surface Map

| URL pattern | Purpose | Confidence |
|---|---|---|
| `/` | Homepage with one-step search | high |
| `/Suche/de/<bundesland>/<city>/wohnen-auf-zeit?...` | Short-term-rental search results | high |
| `/Suche/de/<bundesland>/wohnen-auf-zeit?...` | State-level search (no city slug) | high |
| `/wohnen/wohnen-auf-zeit.html` | Category landing page | medium |
| `/expose/<id>` | Individual listing detail | high |
| `/Suche/de/<bundesland>/<city>/<other-category>` | Other categories (mietwohnungen, häuser-mieten, eigentumswohnungen, ...) | low — only naming pattern observed |

State slugs observed: `berlin`, `brandenburg`. City slugs observed: `berlin`. Pattern uses lowercase, hyphenated; other Bundesländer not tested.

## Confirmed URL Filter Parameters (Wohnen auf Zeit)

| Parameter | Effect | Example |
|---|---|---|
| `fulltext=<keyword>` | Free-text "Eigene Suchkriterien" search | `?fulltext=Seeblick`, `?fulltext=See` |
| `shorttermaccommodationtype=house` | Restrict to Haus | also `flat_or_apartment`, `room`, value-name maps to UI options FLAT_OR_APARTMENT/HOUSE/ROOM |
| `enteredFrom=one_step_search` / `enteredFrom=ai_search` / `enteredFrom=result_list` | Telemetry hint added by site; not a filter | |

**Unknown URL params** (UI options exist, not exercised against URL):
- Pauschalmiete (price range): `totalRentRangeMin`/`totalRentRangeMax` (input names confirmed in form, URL effect not tested)
- Number of rooms: `numberOfRoomsRangeMin`/`numberOfRoomsRangeMax` (input names confirmed)
- Mietdauer (duration): `rentalPeriodValue` (1–6) + `rentalPeriodType` (`Tage`/`Wochen`/`Monate`) — selects exist
- Möblierung: `withFurniture` radio
- Merkmale checkboxes: `onlyWithParking`, `onlyWithGuestToilet`, `onlyWithBasement`, `onlyWithBarrierFree`
- `smokingPermitted` radio, `petsAllowedTypes.NEGOTIABLE/YES/NO`
- `numberOfPersons` (egal/1/2/3)

## Filter Modal — Known Sections (Wohnen auf Zeit only)

| Section | Items | Notes |
|---|---|---|
| Immobilientyp | Wohnung / Haus / Wohnung & Haus / Grundstück / **Wohnen auf Zeit** / WG Zimmer / Garage / Stellplatz / Seniorenwohnen / Pflegeheim / commercial subset | Picking "Wohnen auf Zeit" changes available downstream filters |
| Pauschalmiete | Min/max € | |
| Anzahl der Zimmer | Min/max | |
| Mietdauer | Value (1–6) + unit (Tage/Wochen/Monate) | UI offers down to 1 day |
| Bezugsfrei am | Date | |
| Wohnart | Wohnung / Apartment, Haus, Zimmer | Tied to category |
| Verfügbar für mind. | Min stay duration | |
| Möblierung | alle / möbliert / unmöbliert | |
| Merkmale | Garage/Stellplatz, Gäste-WC, Keller, stufenloser Zugang | "Mehr anzeigen" only added stufenloser Zugang |
| Nichtraucher | radio | |
| Haustiere | NEGOTIABLE/YES/NO checkboxes | |
| Eigene Suchkriterien | Free-text — maps to `fulltext` URL param | E.g. "z.B. Altbau, Kamin" |
| Energieeffizienzklasse | Energy class | Not mapped in detail |

## Listing Card / Results Page

| Element | Selector | Notes |
|---|---|---|
| Listing detail link | `a[href*="/expose/"]` | Each card wraps multiple of these |
| Listing ID | `/expose/(\d+)` | Numeric |
| Headline | `h2` inside the card | Many h2s on page; first h1/h2 also has the result count |
| Result count header | first `h1`/`h2` text "N Angebote für ..." | |

Search bar on results pages has two modes: **KI-Suche** (natural language) and **Klassisch**. KI-Suche did not narrow results in observation — only added `enteredFrom=ai_search`.

## Listing Detail Page — Known Fields (Wohnen auf Zeit)

| Field | Where | Notes |
|---|---|---|
| Title | `h1` | Property title |
| Price | First `1.300 €` block + "Miete/Monat" label | German number format (period as thousand) |
| Rooms / area | "N Zi." / "M m²" | |
| Bezugsfrei ab | Date string | |
| **Mindestmietdauer** | Labeled text "Mindestmietdauer N Monat(e)" | **Often 1–6 months minimum** even when search filter says 1 day |
| Address | "Adresse" block (often partial — "Die vollständige Adresse erhalten Sie vom Anbieter") | |
| Wohnart, Etage, Heizungsart, Baujahr | Tabular details | |
| Description | "Objektbeschreibung" section | German, often long |
| Lage | "Lage" section | Surrounding area description |

## Known Domain Knowledge

- **"Wohnen auf Zeit" listings frequently impose multi-month minimum stays** despite the platform's filter UI offering "1 Tag/1 Woche". Single Potsdam listing observed required 6 months minimum, max 12 months. Treat the platform as poorly suited for true 1–4 week stays unless individual `Mindestmietdauer` is verified.
- Prices in this category are quoted **per month** (`€/M.`) almost universally; only one observed listing showed weekly pricing (`€/W.`).
- Workflow statuses, transitions, and account dashboard surfaces (Mein Bereich, Plus subscription) were not explored.
- Footer "Bundesländer" panel exposes all 16 state landing URLs — useful as authoritative slug source.
- Adjacent regions cross-link from Berlin to Brandenburg districts: Potsdam, Dahme-Spreewald, Potsdam-Mittelmark, Oder-Spree, Havelland, Märkisch-Oderland, Teltow-Fläming, Oberhavel, Barnim.

## Known Gotchas

- **Filter modal scrolls inside its own container**, not the page. To reach lower sections via JS, scroll the modal element (`.Modal_modal-content-wrapper__wBRMO`) directly.
- **Filter modal closes on snapshot in some test runs** — prefer screenshots.
- **`fulltext` keyword is substring-matched** including against location names. `fulltext=See` matches Halensee, Weißensee, etc., not just lake-related listings.
- **`fulltext=Seeblick`** is the most precise water keyword observed (7 hits in Brandenburg vs 47 for `See`).
- **The "6.504 Treffer" element is an `<a>` link**, not a button — interact with it as a link.
- Some autocomplete suggestions inject URL `#/cockpit` after applying a filter via the modal.

## Unknown / Not Tested

- Login flows, posting an inserate, paid Plus features
- Mietwohnungen / Häuser mieten / Eigentumswohnungen URL params (different filter set)
- Map-based search ("Suchgebiet zeichnen")
- Saved-search alerts, Merkzettel
- Mobile site
- Other Bundesländer slugs beyond Berlin/Brandenburg
- Energieeffizienzklasse URL encoding
- Rate limits and bot detection
