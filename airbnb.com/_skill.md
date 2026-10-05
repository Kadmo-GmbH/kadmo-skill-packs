---
name: Airbnb Vacation Rentals
domain: airbnb.com
match:
  - airbnb.com
  - www.airbnb.com
tags: ["@vacation-rental", "@short-term", "@booking", "@travel"]
learned: "2026-05-04"
last_verified: "2026-05-04"
confidence: 0.45
---

# Airbnb Vacation Rentals

Public-facing search and listing surface for short-term vacation rentals. This pack reflects what was observed in a single guided session focused on Berlin/Brandenburg waterfront search — selectors and URL patterns are confirmed for that flow but the rest of Airbnb (host dashboard, messaging, trips, wishlists, payment, account settings) is **not** covered here.

## Browser Access

No login required for browsing or extracting listing data. Logged-in state may surface different prices (member discounts) and saves to wishlist; not yet tested. Some pages return placeholder counts ("Over 1,000 homes") even when filters are applied — extract per-card data instead of trusting the header.

## Known Surface Map

| URL pattern | Purpose | Confidence |
|---|---|---|
| `/<city>--<country>/stays` | Destination landing page with calendar/search overlay | medium |
| `/s/<destination>/homes` | Search results (homes filter applied) | high |
| `/rooms/<id>` | Individual listing detail page | high |
| `/become-a-host`, `/wishlists`, `/trips` | Listed in nav, **not explored** | none |

## Confirmed URL Filter Parameters

Verified in session against live results pages:

| Parameter | Effect | Example |
|---|---|---|
| `amenities[]=64` | Apply **Waterfront** amenity filter | `?amenities%5B%5D=64` |
| `kg_and_tags[]=Tag:686` | Sometimes appended alongside Waterfront — exact role unclear | `?kg_and_tags%5B%5D=Tag%3A686` |
| `room_types[]=Entire home/apt` | Restrict to entire homes (vs. private/shared rooms) | `?room_types%5B%5D=Entire%20home%2Fapt` |
| `min_bedrooms=N` | Minimum bedroom count | `?min_bedrooms=2` |
| `checkin=YYYY-MM-DD` / `checkout=YYYY-MM-DD` | Date window | `?checkin=2026-05-18&checkout=2026-05-25` |
| `adults=N` / `children=N` / `infants=N` / `pets=N` | Guest count | `?adults=2&children=3` |
| `place_id=<google-place-id>` | Geo anchor (Google Place ID) | `place_id=ChIJAVkDPzdOqEcRcDteW0YgIQQ` for Berlin |
| `query=<text>` | Free-text destination | `query=Berlin%2C%20Germany` |

**Unknown / not tested:** other amenity IDs (kitchen, pool, sauna, hot tub, EV charger), accessibility filters, host language, Instant Book, Self check-in, price range, property type drill-down (Houseboat, Cabin, Tiny home), Guest favorite/Luxe filters, sort order parameter.

## Filter Modal — Known Sections

Found via the Filters button in the search results header:

| Section | Items observed | Notes |
|---|---|---|
| Recommended for you | 1+ bathrooms, Allows pets, Washer, Kitchen | Quick-toggle pills |
| Type of place | Any / Home / Room / Hotel | Maps to `room_types[]` |
| Price range | Slider €50–€1800+ histogram | URL param unknown |
| Rooms and beds | Bedrooms, beds, bathrooms | `min_bedrooms` confirmed |
| Amenities | Popular / Essentials / Features / **Location → Waterfront** / Safety | Has "Show more" reveal |
| Booking options | Instant Book, Self check-in, Allows pets | URL params unknown |
| Standout stays | Guest favorite, Luxe | URL params unknown |
| Property type | Collapsed | Items not explored |
| Accessibility features | Collapsed | Items not explored |
| Host language | Collapsed | Items not explored |

## Listing Card — Known Selectors

| Element | Selector | Notes |
|---|---|---|
| Listing card container | `[itemprop="itemListElement"]` | Stable on results page |
| Detail link | `a[href*="/rooms/"]` | Per-card listing link with full search context |
| Listing ID | extracted from `/rooms/(\d+)` | Numeric ID |
| Filters button | `button` with text "Filters" | Opens modal |

## Listing Detail Page — Known Fields

Captured from `body.innerText` on detail pages:

| Field | How to find it | Notes |
|---|---|---|
| Title | `h1` | Property title |
| Capacity | regex `(\d+)\s+guests?\s*·\s*(\d+)\s+bedrooms?` | Compact summary line |
| Listing highlights | "Listing highlights" section | E.g. "On the lake", "Self check-in", "Park view", "Furry friends welcome" |
| Description | Free text under "Some info has been automatically translated. Show original" | Often translated EN; toggle for original DE |
| Sleeping arrangement | "Where you'll sleep" with bedroom blocks | E.g. "Bedroom 1: 1 double bed" |
| Amenities | "What this place offers" + "Show all N amenities" | Includes Waterfront, Lake access, Beach access etc. |
| Reviews | Per-review blocks with rating, date, text | Up to 6 visible without expanding |

## Known Domain Knowledge — Property Categories

| Category text | Meaning |
|---|---|
| `Cottage`, `Bungalow`, `Cabin`, `Tiny home` | Standalone small house |
| `Home` / `Vacation home` | Standalone full house |
| `Houseboat` | On-water structure |
| `Apartment` / `Condo` / `Rental unit` | In a multi-unit building |
| `Room` | Private room in shared property |
| `Hotel` | Hotel listing (separate tab) |

## Known Amenity Distinctions (Water-Related)

Verified by reading detail pages of multiple listings:

| Amenity tag | What hosts actually mean (often) |
|---|---|
| `Waterfront` | Sometimes literal (own dock/jetty), sometimes "near water" — must read description |
| `Lake access` | Implies path or shared access to lake |
| `Beach access` / `Shared beach access` / `Beachfront` | Beach is reachable; not always on the property |
| `Lake view` | View only; no implied access |
| `Pool` | On-property pool |
| `Sauna` | On-property sauna |

**Important calibration**: Listings titled "directly on", "lakeside", "right on the lake" frequently turn out to be 100–300 m walks. Verify against the description text, not the title.

## Known Workflows (Observed Manual Pattern)

| Goal | Manual steps that work |
|---|---|
| Search waterfront homes in a city | Navigate `/s/<dest>/homes?amenities[]=64&checkin=...&checkout=...&adults=...&children=...` then read cards via `[itemprop="itemListElement"]` |
| Verify direct water access | Open `/rooms/<id>`, read description for keywords like "jetty", "dock", "private harbor", "swim from", or distances in meters |
| Cross-reference availability | Open detail URL with `check_in`/`check_out` query params; "unavailable" message appears if dates blocked |

No YAML workflows authored yet for this pack — all observations are starting points, not productionized routines.

## Known Gotchas

- **Header count is unreliable**: results page often shows "Over 1,000 homes" even when filter is active. Trust per-card data and the `Show N places` link in the Filters modal footer.
- **AI search doesn't narrow URL filters**: typing natural-language queries into KI/AI search returns the same broad URL with `enteredFrom=ai_search` — does not visibly translate to URL filter params in observed runs.
- **Marketing language is unreliable**: "directly on", "lakeside", "1st row" can mean 50–300 m walk. Read full description.
- **Pricing varies by guest count**: same listing returns different totals when `adults`/`children` change — re-quote when changing party size.
- **Detail pages: above-the-fold only loads initially**: when scraping `body.innerText` immediately after navigation, you may get only ~1.3 KB. Wait for content or scroll before extracting full description and reviews.

## Unknown / Not Tested

- Login, account, payment, messaging, host inbox, calendar
- Wishlists / saved searches
- Trips management, cancellations, refunds
- Map-based search interactions (drag, zoom, draw area)
- Pricing breakdown modal contents
- Reviews pagination beyond initial 6
- Property-type filter URL params (Houseboat, Cabin, etc.)
- Amenity ID catalog beyond `64=Waterfront`
- Mobile site DOM
- Localization differences (DE vs EN URL patterns and copy)
- API endpoints (only DOM scraping observed)
