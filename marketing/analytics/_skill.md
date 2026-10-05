---
name: Marketing Analytics
domain: marketing
parent: marketing
tags: ["@analytics", "@attribution", "@measurement", "@reporting"]
confidence: 0.40
---

# Marketing Analytics

Measurement, attribution, and optimization frameworks for performance marketing. Covers conversion tracking setup, attribution models, incrementality testing, budget allocation, campaign auditing, and dashboard design. The measurement backbone that all other marketing modules depend on.

## Content Structure

Marketing analytics operates across three layers:

```
Data Collection (tracking, tagging, pixel setup)
    ↓
Data Processing (attribution, deduplication, validation)
    ↓
Data Activation (reporting, optimization, budget allocation)
```

Each layer has distinct concerns. A mistake in collection propagates through everything downstream.

## Key Concepts

### Conversion Tracking Setup

| Component | What | Platform |
|-----------|------|----------|
| **Pixel/tag** | JavaScript snippet on your site that fires on events | Meta Pixel, Google Tag, LinkedIn Insight Tag, TikTok Pixel |
| **Server-side tracking** | API-based event sending (bypasses browser restrictions) | Meta CAPI, Google Server-side GTM, LinkedIn CAPI |
| **Conversion events** | Specific actions you want to measure | Purchase, lead form submit, signup, add to cart |
| **Enhanced conversions** | First-party data matching for better attribution | Google Enhanced Conversions, Meta Advanced Matching |
| **Offline conversions** | Import backend/CRM data back to ad platforms | All major platforms support offline import |

**Tracking hierarchy (install in this order):**
1. Google Tag Manager (container for all tags)
2. GA4 (analytics baseline)
3. Ad platform pixels (Meta, Google Ads, LinkedIn, TikTok)
4. Server-side endpoints (CAPI for Meta, server-side GTM)
5. Enhanced conversions / Advanced Matching
6. Offline conversion import pipeline

### Attribution Models

| Model | How It Works | Best For | Weakness |
|-------|-------------|----------|----------|
| **Last click** | 100% credit to last clicked ad | Direct response, search | Ignores upper funnel |
| **First click** | 100% credit to first touchpoint | Awareness campaigns | Ignores conversion assist |
| **Linear** | Equal credit to all touchpoints | Understanding full journey | Overvalues passive touches |
| **Time decay** | More credit to recent touchpoints | Longer sales cycles | Undervalues discovery |
| **Position-based (U-shaped)** | 40/20/40 to first, middle, last | Balanced view | Arbitrary weighting |
| **Data-driven (DDA)** | ML-assigned credit based on path analysis | Sufficient data (300+ conv/month) | Black box, needs volume |
| **Platform-reported** | Each platform claims credit for its touches | N/A — this is what platforms report | Every platform over-counts |

**The attribution truth hierarchy:**
1. **Backend/CRM data** — actual revenue, actual leads. Ground truth.
2. **Incrementality tests** — what would have happened without the ad? Causal truth.
3. **Media Mix Models (MMM)** — statistical allocation across channels. Directional.
4. **Multi-touch attribution (MTA)** — touchpoint-level credit. Useful but biased.
5. Platform-reported — each platform's self-reported numbers. Always over-counts.

Never use platform-reported numbers alone for budget decisions. Always reconcile with backend data.

**Blind spots all models share:**
- **Viral/referral loops** — PLG products where users invite teams (e.g., Notion, Slack, Figma) generate growth that no attribution model captures. If 40% of signups come from team invites, attribution over-credits paid channels by 40%.
- **Brand halo** — organic search for brand terms is often created by paid awareness campaigns. Last-click gives search 100% credit; the reality is shared.
- **Dark social** — word-of-mouth, Slack/Discord sharing, private messages. Untraceable but often the largest growth channel for PLG products.

### Incrementality Testing

The gold standard for measuring true ad impact. Each method has specific minimum requirements.

| Test Type | How | Min Duration | Min Budget/Scale | Best For |
|-----------|-----|-------------|------------------|----------|
| **Geo lift** | Run ads in some regions, hold out others | 2-4 weeks + cooldown | 10+ geos, 6mo+ history | Channel-level incrementality |
| **Meta Conversion Lift** | Platform holdout (5-20% of audience) | 2-4 weeks | 100+ conv/week, $5K+ annual | Meta campaign incrementality |
| **Google Conversion Lift** | Platform holdout (Bayesian) | 14+ days | $5K+ budget | Google campaign incrementality |
| **On/off test** | Pause channel, measure total impact | 2-4 weeks | Willingness to lose channel revenue | Is this channel incremental at all? |
| **Ghost bidding** | Bid in auction, don't show ad | 2-4 weeks | Programmatic only | Display/programmatic lift |

**Geo lift test design (GeoLift methodology):**
- Minimum geos: 10, ideally 20+ for robust synthetic control
- Pre-test data: 2x the experiment length (e.g., 12-week test needs 6 months history)
- Test markets: between 2 and half of total geos
- Significance level: alpha = 0.1 (90% confidence is standard for geo tests)
- Power target: 80% minimum
- MDE is simulation-derived, not formula-based — run power simulations across effect sizes

**Platform lift study requirements:**

| Platform | Type | Min Budget | Min Conversions | Duration |
|----------|------|-----------|-----------------|----------|
| Meta | Conversion Lift | $5K annual | 100/week | 2-4 weeks |
| Meta | Brand Lift | $120K (US) | N/A (survey) | 2-4 weeks |
| Google | Conversion Lift | $5K | 1,000 total (directional) | 14+ days |
| Google | Brand Lift | $10K (US) | 1.5M impressions | 10+ days |
| TikTok | Brand Lift | $30K (US) | N/A (survey) | 3-4 weeks |

**Power calculation for holdout tests (two-proportion z-test):**
```
n_per_group = (z_α/2 × √(2p̄(1-p̄)) + z_β × √(p₁(1-p₁) + p₂(1-p₂)))² / (p₁ - p₂)²
```
Where p₁ = control CVR, p₂ = expected treatment CVR, z_α/2 = 1.96 (95%), z_β = 0.84 (80% power).

**Practical minimum:** 10,000 users in your smallest group for typical marketing conversion rates (1-4%).

### Campaign Audit Framework

Structured audit across six dimensions:

| Dimension | What to Check | Red Flags |
|-----------|--------------|-----------|
| **Structure** | Campaign/ad group organization, naming, segmentation | Mixed intents in one ad group, no naming convention |
| **Spend efficiency** | CPA/ROAS by segment, wasted spend, budget pacing | >20% spend on non-converting segments |
| **Tracking** | Conversion setup, event firing, data match rate | Platform conversions ≠ backend by >20% |
| **Creative** | Performance by creative, fatigue signals, testing cadence | Same creative running >30 days without test |
| **Audience** | Targeting quality, overlap, exclusions, funnel alignment | No exclusions between funnel stages |
| **Bidding** | Strategy appropriateness, learning phase status, target setting | tCPA target 50% below actual CPA |

### Budget Allocation Framework

**Method 1: Historical performance allocation**
```
Channel budget share = (Channel conversions × Channel CPA efficiency score) / Total weighted conversions
```
Where CPA efficiency score = 1 / (Channel CPA / Average CPA). Channels with below-average CPA get more budget.

**Method 2: Marginal CPA allocation**
- Plot CPA vs spend for each channel (diminishing returns curve)
- Allocate next dollar to the channel with the lowest marginal CPA
- Stop when marginal CPA exceeds target for all channels

**Method 3: Portfolio allocation**
- Set minimum viable spend per channel (below which data is insufficient)
- Allocate remaining budget proportional to ROAS/CPA efficiency
- Reserve 10-15% for testing new channels/campaigns
- Rebalance monthly based on trailing performance

**Starting-point allocation by business model** (adjust with data after 30 days):

| Channel | PLG SaaS | Sales-led B2B | B2C Ecommerce |
|---------|----------|--------------|---------------|
| Paid Search | 30-40% | 25-35% | 35-45% |
| Paid Social (Meta) | 25-35% | 5-10% (retargeting) | 25-35% |
| LinkedIn | 0-5% | 20-30% | 0% |
| Display/Programmatic | 5-10% | 5-10% | 10-15% |
| Content/SEO | 15-20% | 15-20% | 5-10% |
| Email/Nurture | 5% | 5-10% | 5-10% |
| Testing reserve | 10% | 10% | 10% |

Key difference: B2B prioritizes LinkedIn (20-30%) while PLG/B2C prioritizes Meta. Search is always significant but highest for ecommerce (high-intent product searches).

### Measurement Maturity Model

Self-assessment framework — identify current level, build a roadmap to the next.

| Level | Description | Tools | Advancement Criteria |
|-------|-------------|-------|---------------------|
| **1 — Ad Hoc** | Siloed platform data, basic metrics, no cross-channel view | Platform dashboards only | Implement GA4 + UTM taxonomy |
| **2 — Foundational** | Centralized analytics, consistent UTMs, basic reporting | GA4, Looker Studio, UTM builder | Implement multi-touch attribution |
| **3 — Attribution** | MTA active, backend reconciliation, cross-channel view | GA4 DDA, CRM integration, BI tool | Run first incrementality test |
| **4 — Incrementality** | Regular lift tests, causal measurement, test-and-learn culture | Geo-lift tools, platform lift studies | Implement MMM |
| **5 — Modeling** | MMM + incrementality + platform data, triangulated decisions | PyMC-Marketing/Robyn/Meridian, data science team | Continuous optimization loop |

Most accounts should target Level 3 and run periodic Level 4 tests. Level 5 requires dedicated data science and $1M+ annual spend.

### Budget Pacing

| Formula | Calculation | Use |
|---------|-------------|-----|
| **Target Daily Spend** | Monthly budget / days remaining | Daily pacing target |
| **Pacing %** | (Actual spend / Expected spend) × 100 | Over/under-pacing detection |
| **Projected Monthly** | (Spend to date / Days elapsed) × Days in month | End-of-month projection |

**Pacing strategies:**
- **Even pacing** — Spread evenly across the month. Simple but consistently underperforms (Guhl et al., 2025).
- **Waterlevel pacing** — Spend proportional to traffic patterns. Outperforms all heuristics in empirical studies. Formula: `ideal_budget(t) = daily_budget × (traffic_in_period(t) / total_daily_traffic)`
- **Front-loaded** — Spend more early in the month. Use for time-sensitive campaigns or when learning phase matters.

**Day-of-week CPM patterns** (Gupta Media, billions of impressions):
- Weekdays ~1.4% more expensive than weekends on Meta
- Friday is peak CPM on Meta; Thursday is peak on TikTok
- Weekends are 5-7% cheaper on TikTok
- Adjust daily targets by these patterns rather than spending evenly

**Alert thresholds (three-tier):**

| Severity | Threshold | Response |
|----------|-----------|----------|
| **Low** | 10-20% deviation from 30-day rolling baseline | Review within 24h |
| **Medium** | 20-40% deviation OR consistent drift 3+ days | Investigate within 4h |
| **High** | 40%+ deviation, zero conversions, tracking failure | Immediate action |

Use z-scores on 30-day rolling baselines, separated by day-of-week. 2σ = warning, 3σ = critical. Target false positive rate under 30%.

### Dashboard Design

A performance marketing dashboard answers three questions:
1. **Are we on track?** — KPIs vs targets (pace and actual)
2. **What's working?** — Top/bottom performers by channel, campaign, creative
3. **What should we do?** — Trends that require action

**Essential dashboard sections:**

| Section | Metrics | Granularity |
|---------|---------|-------------|
| **KPI scorecard** | Spend, conversions, CPA, ROAS, vs target, vs prior period | Daily/weekly/monthly |
| **Channel breakdown** | Spend, conv, CPA, ROAS by channel | Weekly |
| **Campaign performance** | Top 10 / bottom 10 by primary KPI | Weekly |
| **Creative performance** | CTR, conv rate, CPA by creative | Weekly |
| **Funnel metrics** | Impressions → clicks → visits → conversions (with drop-off rates) | Weekly |
| **Pacing** | Budget spent vs plan, projected end-of-month | Daily |
| **Trend lines** | CPA and ROAS trailing 4 weeks | Daily |

## Inputs & Outputs

**Inputs:**
- KPI targets and budget (from `media-context.md`)
- Platform data exports (Google Ads, Meta, LinkedIn, TikTok)
- Analytics data (GA4, backend)
- CRM/backend conversion data
- Historical performance data

**Outputs:**
- Performance reports (executive summary + detailed)
- Attribution analysis
- Budget allocation recommendations
- Campaign audit findings with prioritized actions
- Tracking audit with fix list
- Dashboard specifications

## Modes

| Mode | What You're Doing |
|------|-------------------|
| **Tracking setup** | Configuring pixels, events, server-side, UTMs |
| **Audit** | Assessing campaign health across six dimensions |
| **Report** | Building performance reports against KPIs |
| **Optimize** | Recommending budget shifts, bid changes, pauses |
| **Attribution** | Analyzing cross-channel credit, designing incrementality tests |

## Common Tasks

1. **Tracking audit** — Verify conversion measurement:
   - Check all pixels fire correctly (use Tag Assistant, Meta Pixel Helper)
   - Compare platform-reported conversions to GA4 to backend
   - Verify UTM parameter consistency across all campaigns
   - Test conversion events on staging before production
   - Confirm server-side tracking is active and matching
   - Document any gaps and their impact on reported numbers

2. **Campaign performance report** — Weekly/monthly report:
   - Pull data from all active platforms
   - Normalize attribution windows for cross-channel comparison
   - Calculate blended and channel-level KPIs
   - Compare vs targets and vs prior period
   - Identify top 3 opportunities and top 3 risks
   - Deliver executive summary + detailed appendix

3. **Budget reallocation** — Optimize spend across channels:
   - Calculate CPA/ROAS efficiency by channel
   - Identify channels with headroom (below target CPA, impression share available)
   - Identify channels at diminishing returns (CPA rising with scale)
   - Propose reallocation with expected impact
   - Set review date to validate reallocation impact

4. **Campaign audit** — Full health check:
   - Score each dimension (structure, spend, tracking, creative, audience, bidding)
   - Prioritize findings by impact (estimated savings or conversion lift)
   - Deliver action items with owner and timeline
   - Schedule follow-up audit in 30 days

5. **Design incrementality test** — Measure true channel lift:
   - Choose test type (geo lift, conversion lift, on/off)
   - Define test and control groups
   - Calculate minimum duration and sample size
   - Set primary metric and minimum detectable effect
   - Plan analysis methodology
   - Document results and implications for budget allocation

## Tips

- Reconcile platform data with backend weekly. Discrepancies grow silently and lead to bad budget decisions.
- Attribution is an opinion, not a fact. Use multiple lenses (platform, MTA, MMM, incrementality) and triangulate.
- Don't optimize a metric you don't trust. If tracking is broken, fix tracking before optimizing campaigns.
- Leading indicators (CTR, CPC, CPM) predict lagging indicators (CPA, ROAS). Monitor both, act on leading indicators early.
- Budget allocation is the highest-leverage optimization. Moving $10K from a 3:1 ROAS channel to a 5:1 ROAS channel creates more value than any single campaign tweak.
- Dashboard design matters. If the dashboard doesn't answer "what should I do differently?" it's just a data display.

## Gotchas

- **Platform conversion counting differences** — Google counts conversions per keyword (can count multiple per click). Meta counts per user within attribution window. LinkedIn uses longer windows. Apples-to-apples comparison requires normalization.
- **View-through attribution inflation** — Display and video campaigns claim view-through conversions liberally. A user who saw a banner and converted via search 7 days later is unlikely a display-driven conversion. Use conservative windows (1-day view-through max) or discount view-throughs.
- **Last-click bias** — Defaulting to last-click attribution makes search and brand look great while making prospecting and display look terrible. This leads to over-investing in bottom-funnel and under-investing in demand generation.
- **Sample size fallacy** — "Campaign A has 50% better CPA than Campaign B" means nothing with 10 conversions each. Use significance calculators before declaring winners.
- **Seasonality blindness** — Comparing this week to last week without accounting for seasonality (holidays, paydays, events) leads to false conclusions. Compare same week last year for seasonal businesses.
- **Vanity metrics** — CTR, engagement rate, and video views feel good but don't pay bills. Always tie analysis back to business-outcome metrics (revenue, qualified leads, LTV).

## References

- `references/attribution-models.md` — detailed model comparison, implementation guides, tool recommendations

## Related Modules

- **paid-search** — search campaign data for attribution
- **paid-social** — social campaign data, platform attribution windows
- **display-programmatic** — view-through attribution, incrementality testing
- **landing-pages** — conversion rate data for full-funnel analysis
