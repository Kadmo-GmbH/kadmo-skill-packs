# Placement Quality — MFA Detection, Scoring & Supply Chain Validation

## MFA Detection Checklist

Score each placement against these signals. Any 3+ flags = high MFA risk, recommend exclusion.

### Signal 1: Ad Density

| Threshold | Risk | Action |
|-----------|------|--------|
| Peak ad density >50% of screen | High — auto-reject | Exclude immediately |
| Average ad density >35% of screen | High | Flag for review |
| Mobile ad density >30% | Violation of Coalition for Better Ads | Exclude |
| Ad density 20-30% | Moderate | Monitor performance |
| Ad density <20% | Normal | No action |

Calculation: Sum heights of all ads within main content portion / total height of main content. Exclude headers, footers, navigation.

### Signal 2: Ad Refresh Behavior

| Indicator | Risk |
|-----------|------|
| Ads auto-refresh while page is in background tab | High |
| Ad refresh interval <30 seconds | High |
| Multiple ad auctions per slot per page view | High |
| 1,000+ impressions served to single user in one session | Critical (Adalytics threshold) |
| Standard refresh (60+ seconds, user-initiated) | Normal |

### Signal 3: Traffic Source Composition

| Traffic Mix | Risk |
|-------------|------|
| >70% paid/arbitrage traffic | High MFA signal |
| >50% paid traffic with low direct/organic | Moderate-High |
| Balanced mix (organic, direct, search, social, paid) | Normal |
| Majority organic/direct traffic | Low risk |

Check via DSP reporting (some DSPs surface traffic source signals) or third-party tools.

### Signal 4: Content Quality

| Indicator | Risk |
|-----------|------|
| AI-generated or syndicated content with no original reporting | High |
| No author attribution, no editorial voice | High |
| Content optimized purely for ad arbitrage (clickbait headlines, slideshow pagination) | High |
| Thin content surrounding excessive ad placements | High |
| Original content with bylines, sources, editorial standards | Normal |

### Signal 5: Site Design & UX

| Indicator | Risk |
|-----------|------|
| Templated/cookie-cutter layout (identical to other MFA sites) | High |
| Aggressive pop-ups, auto-play video with sound, full-screen overlays | High |
| No clear navigation, confusing UX designed to generate accidental clicks | High |
| Professional design with clear content hierarchy | Normal |

## ACT Classification Quick Reference

| Category | Primary Signal | Secondary Signals |
|----------|---------------|-------------------|
| **Arbitrage** | >50% paid traffic sourcing | Usually accompanied by high ad density |
| **Clutter** | Ad-to-content ratio >35% average | Rapidly refreshing placements, MFA layout |
| **Template** | Generic/AI content + cookie-cutter design | Fastest-growing category; harder to detect |

Source: DeepSee.io ACT Framework

## Placement Quality Scorecard

Rate each placement on a 0-100 scale across four dimensions:

| Dimension | Weight | Score Criteria |
|-----------|--------|---------------|
| **Viewability** | 30% | >70% = 100, 50-70% = 70, 30-50% = 40, <30% = 0 |
| **MFA risk** | 25% | 0 MFA signals = 100, 1-2 signals = 50, 3+ signals = 0 |
| **IVT rate** | 25% | <2% = 100, 2-5% = 70, 5-10% = 30, >10% = 0 |
| **Performance** | 20% | Above avg CPA/CTR = 100, avg = 70, below avg = 30, no conversions = 0 |

**Quality Score = weighted average. Action thresholds:**
- 80-100: Allowlist candidate
- 60-79: Monitor monthly
- 40-59: Flag for review, consider exclusion
- <40: Exclude immediately

## Supply Chain Validation

Three-layer transparency check (IAB Tech Lab):

| Layer | What | Check |
|-------|------|-------|
| **ads.txt / app-ads.txt** | Publisher declares authorized sellers | Verify seller IDs match what you see in bid stream |
| **sellers.json** | SSP declares identity of all sellers | Cross-reference with ads.txt entries |
| **SupplyChain Object** | Full path from publisher to buyer | Verify each hop is authorized; fewer hops = lower fraud risk |

Run automated validation weekly. IAB Tech Lab provides validation service. Flag any broken chains or unauthorized resellers.

## IVT Classification (MRC Standard)

| Tier | Type | Detection | Examples |
|------|------|-----------|---------|
| **GIVT** (General) | Routine filtration | Lists, parameter checks | Bots, spiders, crawlers, non-browser user agents, pre-fetch |
| **SIVT** (Sophisticated) | Advanced analytics | Multipoint corroboration | Hijacked devices, adware, malware, misappropriated content |

All MRC-accredited vendors must apply GIVT detection. SIVT detection strongly encouraged.

Benchmark: TAG-certified channels achieve 1.41% IVT rate (88% reduction vs industry average).

## Verification Vendor Selection Guide

| Use Case | Recommended |
|----------|-------------|
| Full-stack brand safety + MFA + IVT | IAS or DoubleVerify (both GARM-aligned, MRC-accredited) |
| MFA-specific detection (web + CTV + mobile) | Pixalate (cross-platform MFA risk scoring) |
| Supply chain validation | IAB Tech Lab automated service |
| IVT-focused detection | HUMAN Security (2,500+ signals per impression, sub-12ms) |
| Attention measurement | IAS (attention metrics) or Adelaide (attention-based media quality) |

**Pre-bid vs post-bid:** Use both layers. Pre-bid blocks known risks before spending. Post-bid discovers new patterns and measures actual placement quality. Pre-bid alone misses novel MFA sites. Post-bid alone wastes spend on known risks.
