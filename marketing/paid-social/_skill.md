---
name: Paid Social
domain: marketing
parent: marketing
tags: ["@paid-social", "@meta-ads", "@linkedin-ads", "@tiktok-ads"]
confidence: 0.36
---

# Paid Social

Campaign management for Meta Ads (Facebook/Instagram), LinkedIn Ads, and TikTok Ads. Covers campaign architecture, audience strategy, creative testing, ad copywriting, bidding, and optimization. Designed for agents that plan, build, and optimize paid social campaigns autonomously.

This module is **platform-agnostic within social**. Platform-specific specs and constraints are in `references/platform-specs.md`. Account-specific details come from `media-context.md`.

## Content Structure

All major social ad platforms share a three-level hierarchy:

```
Campaign (objective, budget type)
└── Ad Set / Ad Group (audience, placement, schedule, bid)
    └── Ad (creative + copy + CTA)
```

| Platform | Campaign Level | Mid Level | Ad Level |
|----------|---------------|-----------|----------|
| Meta | Campaign | Ad Set | Ad |
| LinkedIn | Campaign Group | Campaign | Ad |
| TikTok | Campaign | Ad Group | Ad |

## Key Concepts

### Funnel Architecture

| Stage | Objective | Audience | Creative Approach | KPIs |
|-------|-----------|----------|-------------------|------|
| **Prospecting (TOF)** | Awareness, reach, traffic | Broad interests, lookalikes, Advantage+ | Hook-driven, problem-aware, educational | CPM, CTR, ThruPlay rate |
| **Consideration (MOF)** | Engagement, video views, traffic | Engagers, video viewers, site visitors (7-30d) | Product demos, social proof, comparisons | CPC, engagement rate, VTR |
| **Conversion (BOF)** | Conversions, sales, leads | Retargeting (site visitors, cart abandoners, engaged) | Direct offer, urgency, testimonials | CPA, ROAS, CPL |
| **Retention** | Repeat purchases, upsell | Existing customers, purchasers | New features, cross-sell, loyalty | Repeat purchase rate, LTV |

### Audience Strategy

| Audience Type | What | When to Use | Platform |
|---------------|------|-------------|----------|
| **Broad / Advantage+** | Minimal targeting, algorithm-driven | High volume accounts, 50+ conversions/week | Meta |
| **Interest-based** | Platform-defined interest categories | Early campaigns, small budgets, niche products | Meta, TikTok |
| **Lookalike / Similar** | Modeled from seed audience | Scaling proven audiences (1-5% from purchasers) | Meta, TikTok |
| **Custom audience** | Your data (site visitors, email lists, engagers) | Retargeting, exclusions, lookalike seeds | All |
| **Matched audience** | Uploaded lists, website retargeting | ABM, retargeting | LinkedIn |
| **Firmographic** | Company size, industry, job title, seniority | B2B targeting | LinkedIn |
| **Behavioral** | Purchase behavior, device, app usage | Refined prospecting | Meta, TikTok |

### Lookalike Audience Optimization

**Percentage and performance (Meta, directional benchmarks):**

| LAL % | Reach (US) | CPA vs 1% | Best For |
|-------|-----------|-----------|----------|
| **1%** | ~2.1M | Baseline (lowest) | Direct response, purchase optimization |
| **1-3%** | ~6.3M | +20-30% | Sweet spot for DR at scale |
| **5%** | ~10.5M | +50-70% | Consideration, broader prospecting |
| **10%** | ~21M | +100-200% | Awareness, maximum reach |

**Seed quality hierarchy (most to least effective):**
1. Top 25% customers by LTV (value-based) — 20-40% lower CPA vs all-purchaser seeds
2. All purchasers (value-based lookalike) — 15-30% lower CPA vs binary event seeds
3. All purchasers (binary event) — baseline
4. Add-to-cart users — moderate quality
5. Page visitors — 2-4x worse CPA vs purchaser seeds

**Seed size:** Meta minimum is 100; practical minimum is 1,000-5,000. Optimal range: 5,000-50,000. Above 50K, quality dilution in the seed itself can hurt.

**Refresh cadence:** Every 30-90 days. Website visitor seeds refresh every 14-30 days. Performance degradation typically visible after 60-90 days (rising frequency, declining CTR). Build on auto-updating Custom Audiences to partially mitigate.

**Note:** With Advantage+ and strong pixel data, broad targeting (no lookalike) increasingly matches or outperforms lookalikes. Test broad vs LAL before assuming LAL is always better.

### Audience Layering Rules

1. **Exclude down-funnel audiences** — Retargeting ad sets must exclude converters. Prospecting must exclude retargeting audiences. Otherwise you pay twice for the same user.
2. **Seed quality > seed size** for lookalikes — see hierarchy above.
3. **Refresh custom audiences** — Stale audiences decay. Set rolling windows (30/60/90 days) rather than static lists.
4. **Don't over-narrow** — Stacking 5+ interest layers shrinks the audience below platform optimization thresholds. If audience size is under 100K (Meta) or 50K (LinkedIn), simplify targeting.

### Campaign Budget Strategy: ABO vs CBO

| Strategy | When to Use | How |
|----------|-------------|-----|
| **ABO (Ad Set Budget)** | Testing phase — isolating creative or audience performance | Forces equal spend across ad sets, gives controlled data |
| **CBO (Campaign Budget Optimization)** | Scaling phase — let Meta distribute to top performers | Use once you have validated winners. 3-5 ad sets per CBO campaign. |
| **Hybrid** | Best practice for most accounts | Test with ABO → graduate winners to CBO → top performers into Advantage+ |

Budget threshold: Weekly budget of at least 50x your target CPA for algorithm to have enough data. Winners graduate to Advantage+ Shopping at 10-12 purchases per concept.

### Creative Testing Framework

Structured testing prevents random creative swaps and produces learnable results.

**The 3-3-3 Framework (recommended for Meta, high-volume B2C/PLG):**
- **3 funnel levels** — TOFU / MOFU / BOFU
- **3 concept angles** — different pain points or value propositions
- **3 creative formats** — static / video / carousel (or UGC)
- Creates 27 possible combinations. Test with ABO to force equal spend.
- Run 5-7 days or until 10-12 purchases per concept. Kill lowest performers.

**Scaled-down 2-2-2 variant (for B2B / low-volume / LinkedIn):**
- B2B accounts have lower impression volumes and longer test cycles (2-4 weeks minimum)
- Use 2 funnel levels (prospecting + retargeting), 2 concept angles, 2 formats = 8 combinations
- Run 2-4 weeks per test (not 5-7 days). Need 15+ conversions per variant for LinkedIn.
- LinkedIn minimum: $50/day per campaign for sufficient data. Don't spread thin.

**Testing hierarchy (test one level at a time):**

1. **Concept** — What is the message angle? (problem-focused vs outcome-focused vs social-proof)
2. **Format** — What creative type? (static image, video, carousel, UGC, graphic)
3. **Hook** — What stops the scroll? (first 3 seconds of video, headline, visual pattern interrupt)
4. **Body/Copy** — What's the supporting message and CTA?
5. **Variants** — Minor tweaks within a winning combination (color, text overlay, CTA wording)

**3-Phase testing process (Motion framework):**
1. **Pre-Flight** — Test new creatives against each other only (not against historical winners)
2. **New vs BAU** — Compare phase 1 winners against current best performers
3. **Scaling** — Introduce winners into scaling campaigns, don't pause old creatives

**Creative fatigue signals:**
- CTR declining 20%+ week over week
- Frequency above 3.0 (prospecting) or 7.0 (retargeting)
- CPMr (Cost Per 1,000 Reach) rising above $20 without seasonality
- CPM rising without seasonality explanation
- Action: Introduce new creative, don't just increase budget. Refresh winning creatives every 30-60 days.

### Ad Copy Structure

| Element | Meta/Instagram | LinkedIn | TikTok |
|---------|---------------|----------|--------|
| **Primary text** | 125 chars visible (500 max) | 150 chars visible (600 max) | Overlay text on video |
| **Headline** | 40 chars | 70 chars | N/A (video-native) |
| **Description** | 30 chars (not always shown) | N/A | N/A |
| **CTA button** | Platform presets | Platform presets | Platform presets |

**Copy length and performance (research-backed):**

| Length | CTR | Post-Click CVR | Best For |
|--------|-----|---------------|----------|
| Ultra-short (<80 chars) | Highest | Lower | Brand awareness, impulse, low-price (<$50) |
| Short (80-150 chars) | High | Moderate | Lead gen, app installs |
| Medium (150-300 chars) | Moderate | Moderate-High | Ecommerce, mid-price |
| Long (300-1000+ chars) | Lower | Highest | High-price (>$100), B2B, complex products |

Short copy wins on CTR (~20-25% higher). Long copy wins on CPA for considered purchases (pre-qualifies clickers). Test both — the winner depends on price point and purchase complexity.

**Copy formula (PASO):**
1. **P**roblem — Name the pain in the audience's language
2. **A**gitate — Show the consequence of not solving it
3. **S**olution — Introduce the product as the answer
4. **O**ffer — State the specific CTA and what they get

**Hook formulas (first line, must stop the scroll):**
- Question: "Still doing [painful manual thing]?"
- Stat: "[Number]% of [audience] waste [time/money] on [problem]"
- Contrarian: "Stop [common advice]. Here's what actually works."
- Result: "How [customer] achieved [specific outcome] in [timeframe]"
- Direct: "[Audience]: This is for you."

### Bidding & Budget

| Strategy | When | Platform |
|----------|------|----------|
| **Lowest cost (auto)** | New campaigns, testing phase, broad targeting | Meta, TikTok |
| **Cost cap** | Known CPA target, need cost control | Meta |
| **Bid cap** | Strict CPA ceiling, willing to sacrifice volume | Meta |
| **ROAS target** | Ecommerce, revenue optimization | Meta |
| **Maximum delivery** | LinkedIn (default), budget-constrained | LinkedIn |
| **Advantage Campaign Budget (CBO)** | Multiple ad sets, let algorithm allocate | Meta |
| **Ad Set Budget (ABO)** | Need control per audience segment | Meta |

**Budget rules of thumb:**
- Meta minimum: 2x target CPA per ad set per day (for exit learning phase)
- LinkedIn minimum: $50/day per campaign for sufficient data
- TikTok minimum: $20/day per ad group
- Learning phase: Meta needs ~50 conversions/week per ad set

## Inputs & Outputs

**Inputs:**
- Business goal and KPI targets (from `media-context.md`)
- Audience definition (ICP, existing customer data)
- Creative assets (images, videos, logos)
- Landing page URLs
- Budget and timeline
- Historical performance data (if available)

**Outputs:**
- Campaign architecture (campaigns → ad sets → ads)
- Audience definitions with exclusion rules
- Ad copy per ad set (primary text, headline, description, CTA)
- Creative brief (if assets need production)
- Budget allocation with rationale
- Testing roadmap (what to test, in what order)
- UTM and tracking setup

## Modes

| Mode | What You're Doing |
|------|-------------------|
| **Build** | Creating new campaigns from scratch with full funnel architecture |
| **Creative test** | Designing and analyzing creative tests |
| **Audit** | Reviewing structure, audiences, frequency, creative fatigue |
| **Optimize** | Adjusting bids, budgets, audiences, pausing underperformers |
| **Scale** | Expanding budget on winners, new audience segments, new platforms |

## Common Tasks

1. **Build full-funnel campaign** — Design prospecting + retargeting architecture:
   - Map funnel stages to campaign objectives
   - Define audiences per stage with exclusions
   - Write ad copy per stage (awareness copy ≠ retargeting copy)
   - Plan creative per stage (educational TOF, proof MOF, offer BOF)
   - Set budgets per stage (typically 70% prospecting, 20% retargeting, 10% retention)
   - Configure pixel events and custom conversions

2. **Write social ad copy** — Create ad text for a specific ad set:
   - Understand audience and funnel stage
   - Write 3 primary text variants (short/medium/long)
   - Write 2 headline variants
   - Select appropriate CTA button
   - Ensure message match with landing page

3. **Audit existing creative library** — Before designing new tests, classify what exists:
   - Pull all active ads from Meta/LinkedIn Ad Library
   - Classify each into the 3-3-3 matrix (funnel stage × concept angle × format)
   - Count ads per cell — identify overweight cells (too many variants) and empty cells (untested)
   - Flag stale creative (running 30+ days without refresh)
   - Output: matrix heatmap showing test coverage gaps → these gaps become your next test hypotheses

4. **Design creative test** — Plan a structured test:
   - Define hypothesis (what you're testing and why)
   - Isolate one variable
   - Set success metric and minimum sample
   - Define test duration (minimum 3 days, maximum 14)
   - Plan next action for win/lose/inconclusive

4. **Audience audit** — Review targeting health:
   - Check audience overlap between ad sets (Meta: Audience Overlap tool)
   - Verify exclusion rules are active
   - Review lookalike performance by percentage
   - Check custom audience freshness
   - Assess audience saturation (frequency trends)

5. **Campaign scaling** — Increase spend on winners:
   - Increase budget no more than 20% per day (to avoid resetting learning)
   - Horizontal scaling: duplicate winning ad set with new audience
   - Vertical scaling: increase budget on existing ad set
   - Watch for diminishing returns (CPA increases as you scale)

## Tips

- Creative is the #1 lever in paid social. Better targeting with bad creative loses to broad targeting with great creative.
- Test concepts before polish. A rough UGC video with the right message beats a polished brand video with the wrong message.
- Retargeting audiences are small and burn fast. Keep frequency under 7 and rotate creative every 2 weeks.
- LinkedIn is expensive. CPCs of $8-15+ are normal. Optimize for lead quality, not volume. Use Lead Gen Forms to reduce friction.
- TikTok is creative-first. Ads that look like ads don't work. Native content style outperforms polished brand creative.
- Don't duplicate what's working to "test" it. Duplication creates internal auction competition. Instead, iterate within the winning ad set.

## Gotchas

- **Advantage+ audience expansion** — Meta can expand beyond your defined audience even when you think targeting is narrow. Check actual reach demographics in reporting.
- **Learning phase resets** — Significant edits (budget change >20%, audience change, conversion event change) reset learning. Batch changes and make them during low-spend periods.
- **Attribution window mismatch** — Meta defaults to 7-day click / 1-day view. LinkedIn uses 30-day click / 7-day view. Comparing cross-platform on different windows is misleading.
- **Creative fatigue != ad fatigue** — A tired creative can be refreshed (new thumbnail, new first frame, new headline overlay). You don't always need net-new creative.
- **Lookalike decay** — Lookalike audiences built from old seed data degrade. Refresh seeds quarterly with recent high-value customer data.
- **iOS privacy impact** — App tracking transparency limits Meta's conversion tracking for iOS users. Model conversions may not match backend. Always reconcile with CRM/backend.
- **LinkedIn Audience Network** — Enabled by default, extends ads beyond LinkedIn. Performance often degrades. Disable unless explicitly testing it.

## References

- `references/platform-specs.md` — character limits, image sizes, video specs, placement options per platform
- `references/video-creative-specs.md` — YouTube/TikTok/Reels format comparison, hook framework, modular testing, completion rate benchmarks, video KPI definitions

## Related Modules

- **landing-pages** — conversion optimization for social ad landing pages
- **analytics** — attribution setup for cross-channel measurement
- **paid-search** — query data from search informs social audience creation
