---
name: Display & Programmatic
domain: marketing
parent: marketing
tags: ["@display", "@programmatic", "@retargeting", "@dsp"]
confidence: 0.42
---

# Display & Programmatic

Display advertising and programmatic buying across Google Display Network (GDN), DV360, and third-party DSPs. Covers targeting taxonomy, campaign setup, retargeting strategies, frequency management, viewability, brand safety, and optimization. Designed for agents managing upper-funnel awareness and mid-funnel retargeting through display channels.

## Content Structure

Programmatic campaigns follow a similar hierarchy to search/social but add supply-side concepts:

```
Campaign / Insertion Order (budget, flight dates, KPI)
└── Ad Group / Line Item (targeting, bid, frequency cap)
    └── Creative (banner, native, video, rich media)
        └── Placement (where it runs: site, app, exchange)
```

Key distinction: In programmatic, you buy **impressions** on an exchange, not clicks on a platform. The buyer (DSP) bids in real-time auctions for each impression based on targeting rules.

| Platform | Campaign Level | Targeting Level | Ad Level |
|----------|---------------|-----------------|----------|
| GDN (Google) | Campaign | Ad Group | Ad |
| DV360 | Insertion Order | Line Item | Creative |
| TTD / Other DSP | Campaign | Ad Group | Creative |

## Key Concepts

### Targeting Taxonomy

| Targeting Type | How It Works | Best For | Data Source |
|---------------|--------------|----------|-------------|
| **Contextual** | Match ads to page content (keywords, topics) | Brand-safe prospecting, cookieless-ready | Publisher content signals |
| **Audience — 1st party** | Your own data (site visitors, CRM lists) | Retargeting, lookalikes | Pixel, CRM upload, CDP |
| **Audience — 3rd party** | Purchased audience segments | Prospecting at scale | Data providers (Oracle, Lotame) |
| **Behavioral** | Past browsing/purchase behavior | In-market audiences | Platform signals |
| **Demographic** | Age, gender, income, education | Broad reach campaigns | Platform/publisher data |
| **Geographic** | Country, region, city, radius | Local campaigns, geo-targeting | IP, GPS |
| **Placement** | Specific sites, apps, or channels | Premium inventory, brand safety | Direct deals, PMPs |
| **Retargeting** | Past visitors to your site/app | Conversion campaigns | Pixel/tag data |

### Retargeting Strategies

| Strategy | How | Audience Window | Use Case |
|----------|-----|-----------------|----------|
| **Site retargeting** | All site visitors minus converters | 7-30 days | General re-engagement |
| **Product retargeting** | Viewed specific product/category | 7-14 days | Ecommerce, high-intent |
| **Cart abandonment** | Added to cart, didn't purchase | 3-7 days | Ecommerce, highest intent |
| **Sequential messaging** | Different ads based on engagement depth | 1-90 days in tiers | Nurture sequences |
| **Cross-sell** | Past purchasers, different product | 30-90 days | Existing customer expansion |
| **Suppression** | Exclude recent converters | 7-30 days post-conversion | Prevent waste, improve UX |

### Frequency Management

**Frequency caps by campaign objective:**

| Objective | Weekly Cap | Daily Cap | Notes |
|-----------|-----------|-----------|-------|
| Brand awareness | 3-7/week | 1-2/day | Peak awareness at 7 exposures (Brand Metrics) |
| Consideration | 3-5/week | 2-3/day | Stay top-of-mind during research phase |
| Conversion/retargeting | 5-10/week | 3-4/day | Higher for high-intent; cap at 1-2 week windows |
| B2B long-cycle | 1-2/week | 0.5-1/day | Maintain presence over months |

**Diminishing returns curve:**

| Impressions | Incremental Value | Action |
|-------------|-------------------|--------|
| 1st | Highest marginal return | — |
| 2-3 | Strongest CTR/CVR growth | Recognition building |
| **3-7** | **Sweet spot — 80% of conversion value** | Optimal zone |
| 7-10 | ~15% of conversion value, ROI declining | Monitor closely |
| 10-12 | ~2% incremental value, CPA inflating | Cap reached |
| 12+ | Negative brand perception increases | Waste zone — stop |

**Fatigue diagnostic:** When CTR drops 15-20% from peak over 7 days while frequency rises, creative fatigue is confirmed. Rotate creative, don't just lower frequency.

**Cross-device:** Caps per device = overcounting. A 5/week cap per device means 15/week for a 3-device user. Use people-based frequency via device graphs (deterministic or probabilistic). Cross-channel capping improves effectiveness up to 32% (IAS).

Set frequency caps at the line item level. Campaign-level caps don't prevent clustering within a day.

### Viewability Standards

**MRC/IAB minimum definition:**

| Format | Pixels In-View | Duration | Industry Avg (2024) |
|--------|---------------|----------|---------------------|
| Display banner | 50% | 1 second | ~76% (IAS global) |
| Large format (>242K px) | 30% | 1 second | Higher (larger = more visible) |
| Video | 50% | 2 seconds | ~84% desktop, ~79% mobile (IAS) |

**GroupM premium standard:** 100% of pixels in view. Video requires 50% completion + sound on. This is 2x the MRC baseline and what premium buyers demand.

**Viewability by ad size (benchmark):**

| Size | Avg Viewability | Notes |
|------|----------------|-------|
| 160x600 (skyscraper) | ~67% | Highest among standard units |
| 300x600 (half page) | ~66% | Second highest |
| 300x250 (medium rect) | ~48% | Most inventory, average viewability |
| 728x90 (leaderboard) | ~40% | Often below fold |
| Above-the-fold (any) | ~68% | ATF benchmark |
| Below-the-fold (any) | ~40% | BTF benchmark |

**vCPM formula:** `vCPM = Total Spend / ((Impressions × Viewability%) / 1000)`

Transact on vCPM, not raw CPM. A $3 CPM at 40% viewability = $7.50 vCPM. A $5 CPM at 80% viewability = $6.25 vCPM — the "expensive" inventory is actually cheaper per viewed impression.

### MFA Detection

Made-for-Advertising sites absorb budget with near-zero business value. ANA research: MFA represented 21% of impressions in 2023, now ~6% after industry crackdown — but still present.

**Five consensus MFA signals (ANA/4As/WFA):**
1. High ad-to-content ratio (>30% mobile, >50% desktop)
2. Rapidly auto-refreshing ad placements
3. High percentage of paid traffic sourcing (little organic audience)
4. Generic, templated, or AI-generated non-unique content
5. Poorly designed, templated website layouts

**ACT Classification (DeepSee.io):**

| Category | Signal | Threshold |
|----------|--------|-----------|
| **A**rbitrage | Majority of traffic from paid sources | >50% paid traffic |
| **C**lutter | Excessive ad density | Peak >50% screen = auto-reject; avg >35% = high risk |
| **T**emplate | Generic content + cookie-cutter layout | Often AI-generated |

**Impact:** Ads on MFA sites are 50%+ less likely to drive sales attribution. Non-MFA traffic delivers +278% better conversion rates (IAS).

Detection: Use pre-bid MFA filtering (IAS, DoubleVerify) + review top-spending placements monthly against these signals. See `references/placement-quality.md` for full audit checklist.

### Brand Safety

Uses the **GARM framework** (Global Alliance for Responsible Media): 12 content categories with a Safety Floor (absolute block) and three suitability tiers (Low/Medium/High risk).

| Layer | Timing | Purpose | Trade-off |
|-------|--------|---------|-----------|
| **Pre-bid filters** | Before auction | Block risky inventory before spending | May exclude valid inventory (strict filters) |
| **Post-bid verification** | After ad serves | Discover new patterns, measure actual placement | Money already spent on bad impressions |
| **Exclusion lists** | Config | URLs, apps, categories to never bid on | Requires maintenance |
| **Inclusion lists** | Config | Approved publishers only (premium campaigns) | Limits reach |

**Both pre-bid and post-bid are required.** Pre-bid alone misses edge cases. Post-bid alone wastes spend on known risks.

**Verification vendors** (IAS and DoubleVerify are now effectively a duopoly after Oracle sunset MOAT in 2024):

| Capability | IAS | DoubleVerify |
|------------|-----|-------------|
| Pre-bid MFA filtering | Yes | Yes |
| GARM-aligned categories | Yes | Yes |
| MRC-accredited IVT | Yes | Yes |
| CTV coverage | Yes | Yes |
| Social platform coverage | Yes | Yes |
| Unique strength | Pre-bid MFA + attention metrics | Brand suitability depth + scale |

### Cookieless Targeting Strategy

~47% of the open internet is already cookieless (Safari, Firefox, in-app, CTV). Chrome kept third-party cookies (Google shelved deprecation entirely in 2025 and retired Privacy Sandbox ad APIs in Oct 2025), but regulation and user opt-outs make cookie-only targeting insufficient.

**Strategy hierarchy (prioritized):**

| Strategy | Status | Reach | Best For |
|----------|--------|-------|----------|
| **1. Contextual targeting** | Production-ready | Universal | Prospecting, brand-safe, cookieless-ready |
| **2. First-party data** | Production-ready | Your audience | Retargeting, lookalikes, highest ROI |
| **3. Universal IDs** | Production-ready, partial | Authenticated inventory | Cross-device, CTV |
| **4. Seller-defined audiences** | Maturing (IAB standard) | Publisher inventory | Privacy-safe, publisher-curated |
| **5. Data clean rooms** | Production-ready, growing | Partner overlap | Attribution, audience enrichment |

**Contextual performance:** 41% lower cost per viewable impression vs behavioral targeting (GumGum data). 93% more memorable than mismatched ads. Market at ~$225B (2025), growing 11%+ CAGR.

**Universal ID comparison:**

| Solution | Type | Strength | Limitation |
|----------|------|----------|------------|
| **UID2** (TTD) | Deterministic | Largest ecosystem (200+ partners) | Requires authenticated user |
| **ID5** | Hybrid (deterministic + probabilistic) | 10B+ signals/day | Probabilistic less precise |
| **LiveRamp RampID** | Deterministic | Best offline-to-online bridging | Enterprise pricing |

See `references/targeting-taxonomy.md` for full targeting details.

## Inputs & Outputs

**Inputs:**
- Campaign objective and KPIs (from `media-context.md`)
- Target audience definition
- Creative assets (banner sizes, video, native)
- Budget and flight dates
- Brand safety requirements
- First-party audience data (pixel, CRM)

**Outputs:**
- Campaign/line item structure
- Targeting configuration per line item
- Frequency cap settings
- Creative trafficking specs
- Brand safety configuration
- Pacing schedule
- Reporting cadence and KPIs

## Modes

| Mode | What You're Doing |
|------|-------------------|
| **Build** | New campaign setup — structure, targeting, creative, safety |
| **Retargeting setup** | Audience segmentation, sequential messaging, suppression |
| **Optimize** | Bid adjustments, placement analysis, creative rotation |
| **Audit** | Viewability review, brand safety check, frequency analysis |
| **Report** | Performance reporting, cross-channel comparison |

## Common Tasks

1. **Build display prospecting campaign** — Upper-funnel awareness:
   - Define targeting (contextual + audience layering)
   - Set frequency caps (3-5/week)
   - Configure brand safety filters and exclusions
   - Traffic creative across required sizes (300x250, 728x90, 160x600, 320x50 minimum)
   - Set viewability target (>70%)
   - Configure pacing (even delivery, not front-loaded)

2. **Build retargeting campaign** — Convert known visitors:
   - Segment audiences by intent level (all visitors → category viewers → cart abandoners)
   - Set different bids per segment (highest for cart abandoners)
   - Configure frequency caps per segment
   - Set up burn pixels to suppress converters
   - Plan sequential messaging (awareness → consideration → urgency)

3. **Placement audit** — Review where ads are running:
   - Export placement report
   - Flag low-viewability placements (<50%)
   - Flag high-frequency, low-CTR placements
   - Check for made-for-advertising (MFA) sites
   - Add underperformers to exclusion list
   - Identify top-performing placements for allowlist

4. **Creative rotation** — Manage ad fatigue:
   - Monitor CTR trends per creative
   - Introduce new creative when CTR drops 20%+ from peak
   - A/B test creative concepts (message, visual, format)
   - Ensure all required sizes have fresh creative
   - Retire underperformers (below 0.05% CTR for display)

## Tips

- Programmatic is a reach and frequency game. Optimize for viewable reach at target frequency, not raw impressions.
- Contextual targeting is the privacy-safe future. As cookies deprecate, contextual becomes more important than behavioral for prospecting.
- Start with GDN for simplicity, graduate to DV360/TTD for control. GDN is easier to manage but offers less transparency.
- Creative sizes matter. 300x250 and 320x50 get the most inventory. Missing common sizes means missing auctions.
- PMPs (Private Marketplace deals) offer better inventory quality at higher CPMs. Worth it for brand campaigns where context matters.
- Retargeting is the highest-ROI display tactic. Allocate 60-70% of display budget to retargeting when conversion is the goal.

## Gotchas

- **Made-for-advertising (MFA) sites** — Sites designed to generate ad revenue with minimal content. They inflate impression counts with zero conversion value. Use pre-bid MFA filtering (IAS/DV) and review top-spending placements monthly against the 5-signal checklist. MFA ads are 50%+ less likely to drive attribution.
- **Cross-device frequency** — A frequency cap of 5/week per device means a user on phone + desktop + tablet sees 15 ads. Use deterministic or probabilistic cross-device graphs to manage true frequency.
- **View-through attribution inflation** — Display campaigns often claim conversions via view-through attribution. A user who saw a display ad and later searched your brand and converted is probably a search conversion. Use incrementality tests to measure true display lift.
- **Below-the-fold inventory** — The cheapest display inventory is below the fold. Low CPM ≠ good deal if viewability is 20%. Optimize to vCPM.
- **Retargeting without suppression** — Showing ads to people who already converted wastes budget and annoys customers. Burn pixels should fire on every conversion event.
- **Auto-placements on GDN** — Google includes mobile app placements by default. These often have accidental clicks and poor quality. Exclude `adsenseformobileapps.com` and review app categories.

## References

- `references/targeting-taxonomy.md` — detailed targeting options by platform, audience segment definitions, data provider comparison
- `references/placement-quality.md` — MFA detection checklist, placement scoring rubric, supply chain validation, verification vendor guide

## Related Modules

- **analytics** — view-through attribution, incrementality testing for display
- **paid-search** — search retargeting audiences, cross-channel budget allocation
- **paid-social** — audience sharing, creative reuse across channels
