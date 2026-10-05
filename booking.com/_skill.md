---
name: Booking.com Vacation Stays
domain: booking.com
match:
  - booking.com
  - www.booking.com
tags: ["@booking", "@hotels", "@vacation-rental", "@accommodation", "@travel"]
learned: "2026-05-04"
last_verified: "2026-05-04"
confidence: 0.25
---

# Booking.com Vacation Stays

Global accommodation booking platform. The session touched **only** the search results page and a single vacation-home detail page, with focus on a vacation-home-only filter. Hotels, apartments, hostels, B&Bs, the room-options modal, multi-room booking, reviews tab, host messaging, and the user account are all **not** covered.

## Browser Access

No login required for browsing search results or opening detail pages. Booking shows aggressive anti-bot challenges (`chal_t` query param appears in URL after challenge). Member prices ("Genius") require login. Currency selector affects displayed prices.

## Known Surface Map

| URL pattern | Purpose | Confidence |
|---|---|---|
| `/searchresults.html?ss=<destination>&checkin=...&checkout=...&group_adults=...&group_children=...&age=N&age=N` | Search results | high |
| `/hotel/<country>/<slug>.html` | Property detail page (also used for vacation homes) | medium |
| `/?...` | Homepage with global search | none — not exercised |

Country segment in detail URLs uses 2-letter codes — `/hotel/de/...` for Germany.

## Confirmed URL Filter Parameters

| Parameter | Effect | Notes |
|---|---|---|
| `ss=<text>` | Free-text destination | Use `+` for spaces |
| `checkin=YYYY-MM-DD` / `checkout=YYYY-MM-DD` | Dates | |
| `group_adults=N` | Adult count | |
| `group_children=N` | Children count | |
| `age=N` (repeated) | One per child, with their age | E.g. `&age=8&age=10&age=12` for three children |
| `nflt=ht_id%3D220%3Bdistance_to_water%3D2` | Multi-filter — pipe-encoded | `ht_id=220` = Vacation Home property type; `distance_to_water=2` = within ~2 km of water |
| `chal_t=<n>` / `force_referer=` | Anti-bot challenge tokens — appended automatically | |

**Known property-type IDs (`ht_id`)** observed by name: `220 = Vacation Home`. Other type IDs not enumerated.

**`distance_to_water=N`** — observed values 1 and 2; implied unit is km. Larger values not tested.

## Listing Card / Results Page

| Element | Selector | Notes |
|---|---|---|
| Property card container | `[data-testid="property-card"]` | Stable test ID |
| Title | `[data-testid="title"]` | Property name |
| Price | `[data-testid="price-and-discounted-price"]` | Often "€ N,NNN" formatted |
| Distance / location | Inside the card text — e.g. "20.5 km from Bad Saarow" | No dedicated test ID confirmed |
| Property link | child `<a>` of the card | URL contains `&hapos=`, `&srpvid=`, etc. for tracking |
| Result count | top of page text — `8\nProperties` (English) | Sometimes "no properties found" header is displayed even when results exist below |

## Detail Page — Known Fields

Captured for one Vacation Home (`exklusive-seevilla-berlin-direkt-a.html`):

| Field | Where | Notes |
|---|---|---|
| Title | `h1` / `h2` near top | E.g. "Exklusive Seevilla Berlin direkt am Zeuthener See (Vacation Home) (Germany) Deals" |
| Address | After "Show map" link in body | E.g. "Moßkopfring 31, Treptow-Köpenick, 12527 Berlin, Germany" |
| Capacity / size | "200 m² size", "Max adults: 6" lines | |
| Sleeping | "Bedroom 1: 1 queen bed" lines per bedroom | |
| Amenities list | "Most popular facilities", "Property highlights" | |
| Price | "Includes taxes and fees" total per dates | |
| Review count | "No reviews yet" or "Scored X.X N reviews" near top | Booking review scale is /10 |

## Known Domain Knowledge

- Property categories visible in vacation-home tab: Vacation Home, House, Apartment, Two-Bedroom House, Three-Bedroom House, Five-Bedroom House, Hotel (when not filtered out).
- Booking shows quality rating ("Awarded by Booking.com") for home-style properties, separate from review scores.
- "Limited supply" warning on detail pages ("16 vacation homes like this are already unavailable on our site") indicates demand pressure.
- `Free cancellation` and `Non-refundable` flags are surfaced per rate plan.
- Review scale: 0–10 with band labels (Excellent ≥9.0, Very Good ≥8.0, Good ≥7.0, Pleasant ≥6.0).

## Known Gotchas

- **Anti-bot challenge** can append `chal_t` to URLs. Hard-coded URLs without challenge token still loaded results in this session, but expect intermittent CAPTCHAs.
- **"No properties found" headline can render alongside actual cards** when the destination is too narrow or the filter combination is uncommon. Verify by counting `[data-testid="property-card"]` rather than reading the header.
- **Children require both `group_children=N` and `age=N` repeated for each child** — without ages the search may silently ignore the children count.
- **Pricing varies with party composition** — re-quote if guest count or ages change.
- **Detail-page URL captured from search results includes long tracking params** (`aid`, `label`, `srpvid`, etc.). These can be stripped to the bare `/hotel/<country>/<slug>.html` for canonical reference, but you may lose the dated price quote.

## Unknown / Not Tested

- Login (Genius rates), saved properties, trips, messaging, payment
- Hotel category filters (star rating, breakfast included, free WiFi, etc.)
- Map-based search and zoom interactions
- Multi-room / multi-traveler booking flows
- Property-type ID catalog beyond `220 = Vacation Home`
- Sort orders (popularity, price, distance)
- Pagination
- Reviews tab and review filtering
- Mobile site DOM
- Localization beyond DE/EN URL patterns
- Booking confirmation flow
- Cancellation flows
