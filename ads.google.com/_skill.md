---
name: Google Ads Search Platform
domain: ads.google.com
match:
  - ads.google.com
tags: [advertising, ppc, search-ads, sem, google-ads]
confidence: 0.85
learned: "2026-04-15"
last_verified: "2026-04-15"
---

# Google Ads Search Platform

Browser-based management of Google Ads Search campaigns. Covers campaign diagnostics, responsive search ad editing, keyword management, bid strategy configuration, and ad strength optimization.

## Browser Access

- Requires Google account login in connected browser
- URL pattern: `https://ads.google.com/aw/{section}?ocid={account_id}&authuser={n}`
- Multi-account: `authuser=0` for primary, `authuser=1` for secondary
- Session expires after ~30min idle — watch for account chooser screen
- Ad blocker dialog may appear — does not block functionality but hides some content

## URL Patterns

| Page | URL Pattern |
|------|-------------|
| Campaigns | `/aw/campaigns?ocid={id}` |
| Ad groups | `/aw/adgroups?campaignId={cid}&ocid={id}` |
| Ads | `/aw/ads?ocid={id}` |
| Keywords | `/aw/keywords?campaignId={cid}&ocid={id}` |
| Ad Preview & Diagnosis | `/aw/diagnostic/AdPreview?ocid={id}` |
| Keyword Planner | `/aw/keywordplanner/home?ocid={id}` |
| Campaign Settings | Edit via gear icon on campaign row, not a separate URL |
| RSA Editor | `/aw/ads/edit/search?adId={aid}&adGroupIdForAd={agid}&ocid={id}` |

## Navigation Hierarchy

```
Account
 > Campaign (budget, bid strategy, networks, locations)
   > Ad Group (keyword theme, default bid)
     > Keywords (match type, status, quality score)
     > Ads (responsive search ad: headlines, descriptions, sitelinks)
```

## Key UI Elements

### Top-level Navigation (Left sidebar)

| Element | Location |
|---------|----------|
| Campaigns menu | `menuitem "Campaigns"` — expands to: Campaigns, Ad groups, Ads, Assets, Keywords |
| Tools menu | `menuitem "Tools"` — expands to: Keyword Planner, Ad Preview, etc. |
| Settings tab | Tab within campaign view — campaign-level settings |

### Campaign List Page

| Element | How to Find |
|---------|-------------|
| Campaign link | `a:has-text("Campaign Name")` in grid |
| Edit settings gear | `button "Edit settings"` next to campaign name (opens slide-out panel) |
| Bid strategy column | Column "Bid strategy type" — shows "Maximize clicks", "Manual CPC", etc. |
| Budget column | Column "Budget" — shows daily/total amount |
| Status column | Shows "Bid strategy learning", "Eligible", etc. |

### Campaign Settings Panel (Slide-out)

Opened by clicking gear icon next to campaign name. Contains collapsible sections:

| Section | Key Fields |
|---------|-----------|
| Campaign name | Text input |
| Campaign status | Enabled/Paused |
| Networks | Google Search, Search Partners, Display Network |
| Budget | Daily amount or campaign total |
| Bidding | Strategy type + Max CPC bid limit |
| Locations | Geographic targeting |
| Languages | Language targeting |

**Bidding section expanded** shows:
- Strategy selector (Maximize clicks, Manual CPC, Target CPA, etc.)
- "Set a maximum cost per click bid limit" checkbox
- Maximum CPC bid limit input (e.g., "€2.00")

### Keywords Page

| Element | How to Find |
|---------|-------------|
| Keyword list | Grid with columns: Keyword, Match type, Ad group, Status, Impr., Clicks, etc. |
| Match types | "Phrase match", "Exact match", "Broad match" |
| Status values | "Eligible", "Paused", "Below first page bid" |
| Pagination | "Show rows" dropdown + "Go to next page" button at bottom |

### Responsive Search Ad Editor

Opened by clicking ad title link, then Edit assets FAB button.

| Element | Selector / How to Find |
|---------|------------------------|
| Final URL | `textbox "Final URL"` |
| Display Path 1 | `textbox "Path 1"` |
| Display Path 2 | `textbox "Path 2"` |
| Headlines (max 15) | Series of `textbox "Headline"` inputs |
| Descriptions (max 4) | Series of `textbox "Description"` inputs |
| Business name | `textbox "Business name"` |
| Sitelinks section | Region with "Sitelinks" heading — "Add" buttons for each slot |
| Ad Strength indicator | `progressbar "Ad strength overall score is ..."` |
| Save button | `material-button:has-text("Save ad")` — at very bottom, may need JS to find |
| Pin options | `button "Please select pin options"` next to each headline/description |

**Ad Strength categories** (shown as notes with score indicators):
1. Add more headlines
2. Include popular keywords in your headlines
3. Make your headlines more unique
4. Make your descriptions more unique
5. Add more sitelinks

**Score levels**: low (empty circle), medium (blue partial), high (green check)

**Ad Strength ratings**: Incomplete → Poor → Average → Good → **Excellent**

### Sitelink Creation Dialog

Opened by clicking "Add" button in sitelinks section of RSA editor.

| Field | Max Length |
|-------|-----------|
| Sitelink text | 25 chars |
| Description line 1 | 35 chars |
| Description line 2 | 35 chars |
| Final URL | Full URL |

- Supports up to 6 sitelinks per ad
- Multiple sitelinks can be created in one dialog (expandable panels)
- Click "Create" button to save sitelinks, then "Save ad" for the full ad

### Ad Preview & Diagnosis Tool

| Element | How to Find |
|---------|-------------|
| Search term input | `combobox "Enter a search term"` |
| Location selector | Region "Location setting" |
| Language selector | Region "Language setting" |
| Device selector | Region "Device setting" |
| Results grid | Grid "Rejected ads" with columns: Campaigns, Ad group, Reason |

**Diagnosis reasons**:
- "The ad is rejected early on because it's unlikely to lead to an impression based on the predicted Ad Rank."
- "Your ad is showing" (green)
- "Your ad isn't showing" (red) with match count

### Keyword Planner

| Element | How to Find |
|---------|-------------|
| Discover keywords card | `button "Discover new keywords"` |
| Search volume card | `button "Get search volume and forecasts"` |
| Keywords textarea | `textbox "Enter or paste your keywords..."` |
| Submit button | Unnamed button next to textarea (becomes enabled when text entered) |
| Results grid | Grid with columns: Keyword, Avg. monthly searches, Competition, Top of page bid (low/high) |

## Data Model

### Campaign

| Field | Type | Values |
|-------|------|--------|
| Status | enum | Enabled, Paused, Removed |
| Bid strategy | enum | Maximize clicks, Manual CPC, Target CPA, Target ROAS, Maximize conversions |
| Max CPC bid limit | currency | Optional cap on auto-bidding strategies |
| Budget type | enum | Daily, Campaign total |
| Networks | multi-select | Google Search, Search partners, Display Network |
| Campaign type | enum | Search, Performance Max, Display, Video, Shopping |

### Keyword

| Field | Type | Values |
|-------|------|--------|
| Match type | enum | Broad, Phrase, Exact |
| Status | enum | Eligible, Paused, Below first page bid, Removed |
| Quality Score | 1-10 | Based on expected CTR, ad relevance, landing page experience |
| Max CPC | currency | Keyword-level bid override (optional) |

### Responsive Search Ad

| Field | Limits |
|-------|--------|
| Headlines | 3-15 (minimum 3 required, 15 max, aim for 13+ for Excellent) |
| Descriptions | 2-4 (minimum 2 required, 4 max) |
| Final URL | 1 required |
| Display path | 2 segments, 15 chars each |
| Sitelinks | 0-6 (need 6 for "high" score) |
| Business name | 0-25 chars |

## Ad Rank Formula

```
Ad Rank = Bid x Quality Score x Expected Impact of Extensions
```

**Quality Score components:**
1. **Expected CTR** — predicted click-through rate
2. **Ad relevance** — how closely ad matches search intent
3. **Landing page experience** — page relevance, speed, mobile-friendliness

**Key insight**: Low Max CPC bid limit is the #1 cause of "ad rejected early" — check Keyword Planner for "Top of page bid" to set appropriate limits.

## Common Tasks

### 1. Diagnose Why Ads Aren't Showing

1. Navigate to Ad Preview & Diagnosis: Tools > Troubleshooting > Ad preview and diagnosis
2. Enter target keyword in search box
3. Set location, language, device, audience
4. Check results grid — expand each campaign row to see rejection reason
5. If "low Ad Rank": check Max CPC bid limit vs Keyword Planner top-of-page bids

### 2. Check/Update Max CPC Bid Limit

1. Navigate to Campaigns page
2. Click gear icon "Edit settings" next to campaign name
3. Click "Bidding" section to expand
4. Check "Set a maximum cost per click bid limit" checkbox state
5. If checked, compare limit value against Keyword Planner "Top of page bid (high range)"
6. Increase or uncheck to let Google optimize freely
7. Click Save

### 3. Get Keyword CPC Estimates

1. Navigate to Tools > Planning > Keyword Planner
2. Click "Get search volume and forecasts"
3. Enter keywords (comma-separated or one per line)
4. Click submit button (unnamed, next to textarea)
5. Review grid: Avg. monthly searches, Competition, Top of page bid (low/high range)
6. Compare against current Max CPC bid limit

### 4. Improve Ad Strength to Excellent

**Headlines (need 13-15 for Excellent):**
- Include primary keywords in at least 3 headlines
- Mix benefit statements ("Ship Code 20x Faster") with feature statements ("AI Code Review & PRs")
- Include pricing ("From $99/Agent/Month")
- Include CTA ("Deploy AI Agents Today")
- Include trust ("Open Source AI Platform", "No Vendor Lock-In")
- Each headline must be unique — no near-duplicates

**Descriptions (need 4/4):**
- Description 1: core value proposition
- Description 2: features and social proof
- Description 3: metrics and urgency
- Description 4: differentiation and trust (open source, no lock-in)
- Max 90 chars each

**Sitelinks (need 6 for high score):**
- Pricing page link
- How it works / process
- FAQ
- Primary CTA (Get Started / Contact)
- Feature page (tech stack, integrations)
- Trust page (open source, security)
- Each needs: text (25 chars), 2 description lines (35 chars each), URL

**Business name**: Set to match domain or brand

### 5. Cross-Reference with Keyword Planner

1. Export campaign keywords from Keywords page
2. Enter all keywords in Keyword Planner
3. Compare "Top of page bid (high range)" against Max CPC limit
4. Any keyword where bid high > Max CPC will likely not show
5. Prioritize keywords by: search volume x affordability

### 6. Edit Responsive Search Ad

1. Navigate to Ads page
2. Click ad title link to open assets view
3. Click Edit assets FAB button (`material-fab[aria-label="Edit assets"]`)
4. Edit headlines, descriptions, display path, business name
5. Add sitelinks via "Add" buttons in Sitelinks section
6. Monitor Ad Strength indicator — aim for all 5 categories "high"
7. Find Save button: `material-button:has-text("Save ad")` (at bottom, may require JS evaluation to locate)

## States

| State | Indicators |
|-------|-----------|
| Ad showing | Green status in Ad Preview, impressions > 0 |
| Ad not showing — low Ad Rank | "rejected early" in diagnosis, 0 impressions |
| Ad not showing — paused | Campaign or ad group status = Paused |
| Bid strategy learning | Status column shows "Bid strategy learning" (normal for new campaigns, 1-2 weeks) |
| Account not verified | Advertiser verification required banner |
| Session expired | Google account chooser screen appears |

## Tips

- **snapshot(includeContent=true)** is the primary tool — returns full page content + accessibility tree with element refs
- Use **screenshot** only for visual verification (ad preview, chart readings)
- Campaign settings panel is a **slide-out dialog**, not a separate page — click gear icon to open
- The RSA editor "Save ad" button is often **off-screen** — use JS evaluation to find: `material-button:has-text("Save ad")`
- When viewing keyword data, the page may show "1-10 of many" — use pagination buttons to see all
- **Keyword Planner results are region-specific** — check the location filter (Germany vs US will show different volumes/CPCs)
- Google may auto-suggest sitelinks — accept good ones, add custom ones for the rest
- **Display path** is cosmetic only — it doesn't need to match a real URL, but should match the landing page theme

## Gotchas

- **Ad blocker dialog**: "Turn off ad blockers" dialog appears frequently — it does NOT block functionality, just overlays. Most actions still work.
- **Session expiry**: Google Ads sessions expire after ~30min idle. Watch for account chooser screen. Navigate back to the target URL with `authuser=0`.
- **Wrong account login**: If account chooser appears, clicking wrong account (e.g., `agent@domain.com` instead of main account) leads to "You do not have any Google Ads accounts" — must switch back.
- **Save button hidden**: The RSA editor "Save ad" button is a `material-button` at the very bottom of a long scrollable form. Use JS to find it: `document.querySelectorAll('material-button').filter(b => b.textContent.includes('Save ad'))`
- **Preview vs Save confusion**: The RSA editor has both "Preview ads" and "Save ad" buttons at the bottom — they look similar. "Preview" opens a modal, "Save" saves the ad.
- **Sitelink character limits**: Sitelink text = 25 chars, Description lines = 35 chars each. Exceeding causes validation error (red border, "Value too long").
- **Display path not a URL**: Display path is cosmetic (`example.com/ai-agents`) — the actual click destination is the Final URL. Don't put an anchor route here.
- **Settings tab filter**: The "Settings" tab on campaigns page may show "No campaigns match your filters" if campaign-type filters are active.
- **Keyword Planner creates plans**: Each "Get search volume" query creates a persistent "Plan" in your account — clean up old plans periodically.
- **"Bid strategy learning"**: New campaigns show this status for 1-2 weeks. Normal behavior, but if Max CPC is too low, learning phase produces no data.
- **Display Network on Search campaigns**: Enabling Display Network on a Search campaign dilutes budget on low-quality placements — generally disable it for Search campaigns.
- **Hash URLs for sitelinks**: URLs like `https://example.com/#pricing` work as sitelink Final URLs — Google follows the hash anchor.

## References

- `references/ad-strength-checklist.md` — step-by-step checklist for reaching Excellent ad strength
- `references/keyword-diagnostics.md` — diagnostic workflow for "ads not showing" issues
