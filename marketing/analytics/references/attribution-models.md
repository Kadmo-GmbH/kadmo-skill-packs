# Attribution Models — Detailed Guide

Comparison of attribution approaches, implementation guidance, and decision frameworks for selecting the right model.

## Model Comparison Matrix

| Model | Type | Data Needs | Accuracy | Coverage | Best For |
|-------|------|-----------|----------|----------|----------|
| Last click | Rule-based | Minimal | Low | Single channel | Simple funnels, search-heavy |
| First click | Rule-based | Minimal | Low | Single channel | Awareness measurement |
| Linear | Rule-based | Touch data | Medium-low | Multi-channel | Equal-weight exploration |
| Time decay | Rule-based | Touch data | Medium | Multi-channel | Long sales cycles |
| Position-based | Rule-based | Touch data | Medium | Multi-channel | Balanced view |
| Data-driven (DDA) | Algorithmic | 300+ conv/mo | Medium-high | Multi-channel | Sufficient data volume |
| Media Mix Modeling | Statistical | 2+ years data | High (directional) | All channels (inc. offline) | Budget allocation |
| Incrementality | Experimental | Test design | Highest | Per-channel | Causal measurement |

## Platform Attribution Defaults

| Platform | Default Window | Click | View |
|----------|---------------|-------|------|
| Google Ads | 30-day click | Yes | Yes (GDN/YouTube) |
| Meta Ads | 7-day click, 1-day view | Yes | Yes |
| LinkedIn | 30-day click, 7-day view | Yes | Yes |
| TikTok | 7-day click, 1-day view | Yes | Yes |
| GA4 | 90-day (data-driven) | Yes | No (by default) |

### Why Platforms Over-Count

Each platform sees only its own touchpoints and claims credit for any conversion within its window:

```
User journey: LinkedIn ad (day 1) → Google search (day 5) → Meta retarget (day 6) → Purchase (day 7)

LinkedIn reports: 1 conversion (30-day click window)
Google reports:  1 conversion (30-day click window)
Meta reports:    1 conversion (7-day click window)
Backend truth:   1 actual conversion
```

Result: 3 reported conversions for 1 actual purchase. This is normal, not a tracking error.

**Solution:** Never sum platform-reported conversions. Use backend data as the denominator and platform data to understand relative contribution.

## Data-Driven Attribution (DDA)

### How It Works
- Analyzes all converting and non-converting paths
- Uses Shapley value or Markov chain to assign fractional credit
- Accounts for the marginal contribution of each touchpoint
- Requires sufficient data volume to build reliable models

### Requirements
- Google Ads DDA: 300+ conversions and 3,000+ ad interactions in 30 days
- GA4 DDA: Active by default, quality depends on data volume
- Meta: Not available as a formal model; uses modeled conversions

### Limitations
- Black box — you can't see the exact credit logic
- Biased toward channels with more touchpoints (favors display over search)
- Doesn't capture offline, word-of-mouth, or organic brand lift
- Changes over time as user behavior changes

## Media Mix Modeling (MMM)

### What It Is
Statistical model that estimates the contribution of each marketing channel to an outcome (revenue, conversions) using historical aggregate data. Includes non-digital channels (TV, print, OOH) and external factors (seasonality, economy).

### When to Use
- $500K+/year ad spend across 3+ channels
- 2+ years of historical spend and outcome data
- Need to measure channels that aren't clickable (TV, OOH, podcast)
- Budget allocation decisions across channels

### Key Components
```
Revenue = Base + Channel_1_contribution + Channel_2_contribution + ... + Seasonality + External_factors + Error
```

- **Base:** Revenue that would occur with zero marketing
- **Channel contributions:** Each channel's estimated impact
- **Adstock/carryover:** How long a channel's impact lasts after spend stops
- **Saturation:** Diminishing returns as spend increases
- **External factors:** Seasonality, promotions, economic indicators

### Open-Source MMM Tools

| Tool | By | Language | Strengths |
|------|-----|----------|-----------|
| **Meridian** | Google | Python | Bayesian, incorporates geo-level data |
| **Robyn** | Meta | R | Automated hyperparameter tuning, multi-objective |
| **LightweightMMM** | Google | Python | Bayesian, built on Numpyro |
| **PyMC-Marketing** | PyMC Labs | Python | Flexible Bayesian framework |

## Incrementality Testing Guide

### Geo Lift Test

**Setup:**
1. Select test markets (exposed to campaign) and control markets (no campaign)
2. Markets must have similar baseline conversion rates
3. Run for 2-4 weeks minimum
4. Measure conversion lift: (test - control) / control

**Design rules:**
- Minimum 5 test + 5 control markets for statistical power
- Markets should be non-adjacent (prevent spillover)
- Hold other marketing constant during test
- Run pre-test period to establish baseline

### Conversion Lift Test (Platform-Native)

**Meta Conversion Lift:**
- Meta splits eligible audience into test (see ads) and holdout (don't see ads)
- Measures incremental conversions caused by the campaign
- Requires: sufficient budget, Pixel/CAPI, 1-4 week minimum
- Limitations: only measures Meta's contribution, not cross-channel

**Google Conversion Lift:**
- Similar holdout methodology for YouTube and Display
- Available through Google Ads representative
- Requires significant spend ($50K+ recommended)

### On/Off Test

The simplest incrementality test:
1. Pause a channel completely for 2-4 weeks
2. Measure impact on total conversions (all channels)
3. Restart and measure recovery

**When to use:** When you suspect a channel is getting attribution credit it doesn't deserve. If pausing search retargeting has zero impact on total conversions, it wasn't incremental.

**Limitations:** Blunt instrument. Doesn't tell you the optimal spend level, just whether the channel matters at all.

## Attribution Decision Framework

```
Q: What decision are you making?

A1: Which channel gets next $10K of budget?
    → Use: Marginal CPA analysis + incrementality test

A2: How should we split annual budget across channels?
    → Use: MMM (if data exists) or historical ROAS + incrementality

A3: Which campaign within a channel should I scale?
    → Use: Platform DDA + backend conversion reconciliation

A4: Is this new channel worth testing?
    → Use: Geo lift test or on/off test after initial pilot

A5: Should we cut this underperforming channel?
    → Use: On/off test to measure true incremental impact before cutting
```

## Measurement Maturity Model

| Level | Description | Typical Setup |
|-------|-------------|---------------|
| **1 — Basic** | Platform-reported metrics only | Each platform tells its own story, no reconciliation |
| **2 — Tracking** | Centralized analytics + UTMs | GA4 as source of truth, consistent UTM taxonomy |
| **3 — Attribution** | Multi-touch attribution, backend reconciliation | MTA tool or DDA, platform data reconciled with CRM |
| **4 — Incrementality** | Regular lift tests, experimental measurement | Quarterly incrementality tests per major channel |
| **5 — Modeling** | MMM + incrementality + platform data, triangulated | All three lenses informing budget allocation |

Most accounts should target Level 3 and run periodic Level 4 tests. Level 5 requires dedicated data science resources and $1M+ annual spend to justify the investment.

## UTM Taxonomy

Consistent UTM parameters are the foundation of cross-channel measurement.

```
utm_source   = platform name         (google, meta, linkedin, tiktok)
utm_medium   = traffic type           (cpc, cpm, paid-social, email, organic)
utm_campaign = campaign name          (must match campaign naming convention)
utm_content  = ad set / ad group      (audience or creative identifier)
utm_term     = keyword or targeting    (search term or audience segment)
```

**Rules:**
- Lowercase only. `Google` ≠ `google` in analytics.
- No spaces. Use hyphens or underscores.
- Be consistent across all channels and all team members.
- Use a UTM builder template, not manual entry.
- Validate UTMs fire correctly before scaling spend.
