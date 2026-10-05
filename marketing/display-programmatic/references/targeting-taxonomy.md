# Targeting Taxonomy — Display & Programmatic

Detailed targeting options, audience segment definitions, and platform-specific capabilities.

## Targeting Methods by Platform

| Method | GDN | DV360 | TTD | Xandr |
|--------|-----|-------|-----|-------|
| Contextual (keyword) | Yes | Yes | Yes | Yes |
| Contextual (topic/category) | Yes | Yes | Yes | Yes |
| In-market audiences | Yes | Yes | Yes (via 3P) | Yes (via 3P) |
| Affinity audiences | Yes | Yes | Limited | Limited |
| Custom intent | Yes | Yes (custom affinity) | Similar | Similar |
| 1st party (pixel) | Yes | Yes | Yes | Yes |
| 1st party (CRM) | Via Customer Match | Yes | Yes | Yes |
| 3rd party segments | Limited | Extensive | Extensive | Extensive |
| Lookalike/Similar | Yes | Yes | Yes | Yes |
| Geo (country/region/city) | Yes | Yes | Yes | Yes |
| Geo (radius/hyperlocal) | Yes | Yes | Yes | Yes |
| Device | Yes | Yes | Yes | Yes |
| Day/time | Yes | Yes | Yes | Yes |
| Placement (domain) | Yes | Yes | Yes | Yes |
| Placement (app) | Yes | Yes | Yes | Yes |
| Deal ID (PMP) | Limited | Yes | Yes | Yes |
| Programmatic Guaranteed | Yes | Yes | Yes | Yes |

## Audience Segment Categories

### In-Market Audiences (Google Taxonomy)

Users actively researching or planning to purchase in a category. High-intent, lower reach.

Key categories: Autos & Vehicles, Business Services, Consumer Electronics, Education, Financial Services, Home & Garden, Real Estate, Software, Travel.

Example: "In-market for CRM Software" targets users who have recently searched for, compared, or engaged with CRM-related content.

### Affinity Audiences

Users with long-term interests, regardless of current purchase intent. Lower-intent, higher reach.

Example: "Technology Enthusiasts" targets users with ongoing tech engagement (news, product reviews, tech forums).

### Custom Segments

Build your own audiences based on:
- **Keywords** — people who search for these terms
- **URLs** — people who browse sites like these
- **Apps** — people who use apps like these

Most flexible targeting method. Combine keywords + URLs for tight intent targeting.

## Third-Party Data Providers

| Provider | Strengths | Coverage | Use Case |
|----------|-----------|----------|----------|
| **Oracle / Grapeshot** | Purchase intent, demographics | Global | B2C prospecting |
| **Lotame** | Cross-device, data management | Global | Audience unification |
| **Bombora** | B2B intent data (surge signals) | Global B2B | B2B display prospecting |
| **Dun & Bradstreet** | Firmographics, company data | Global B2B | ABM display campaigns |
| **IRI / NCSolutions** | CPG purchase data | US | CPG, retail |
| **Experian** | Demographics, financial | US/UK | Financial services, auto |

Note: Third-party cookie deprecation reduces effectiveness of behavioral segments. Prioritize contextual, 1st party, and publisher 1st party data.

## Retargeting Segment Definitions

| Segment | Definition | Window | Priority |
|---------|-----------|--------|----------|
| **Cart abandoner** | Added to cart, no purchase | 3-7 days | Highest |
| **Product viewer** | Viewed product page, no cart | 7-14 days | High |
| **Category browser** | Viewed category page, no product | 14-30 days | Medium |
| **Site visitor** | Any page visit, no deep engagement | 14-30 days | Lower |
| **Blog reader** | Read content, no product pages | 30-60 days | Lowest (nurture) |
| **Past purchaser** | Completed purchase | 30-90 days | Cross-sell/upsell |
| **Lapsed customer** | No purchase in >90 days | 90-180 days | Re-engagement |

### Segment Exclusion Rules

```
Cart abandoner    → exclude: purchasers (7d)
Product viewer    → exclude: cart abandoners, purchasers (7d)
Category browser  → exclude: product viewers, cart abandoners, purchasers (14d)
Site visitor      → exclude: category browsers, product viewers, cart abandoners, purchasers (30d)
Past purchaser    → exclude: recent purchasers (30d), active retargeting segments
```

## Contextual Targeting Strategy

With cookie deprecation, contextual becomes critical for prospecting.

### When to Use Contextual vs Audience

| Signal | Contextual | Audience |
|--------|-----------|----------|
| Cookie availability | Works without cookies | Requires cookies or login |
| Privacy compliance | Fully compliant | Requires consent |
| Upper funnel | Strong (reach + relevance) | Good (reach, less relevant) |
| Lower funnel | Weak (no user history) | Strong (retargeting) |
| Measurement | Harder to attribute | Easier to attribute |

### Contextual Targeting Methods

1. **Keyword contextual** — Target pages containing specific keywords. Most precise.
2. **Topic/category contextual** — Target pages classified under topics. Broader reach.
3. **Semantic contextual** — AI-classified page meaning, beyond keywords. Most sophisticated.
4. **Custom contextual** — Combine keywords + topics + exclusions for tight control.

### Brand Safety Categories (IAB)

Standard exclusion categories for display campaigns:

| Category | Risk | Default Action |
|----------|------|---------------|
| Adult content | High | Exclude |
| Arms & ammunition | High | Exclude |
| Crime | High | Exclude |
| Death & injury | High | Exclude |
| Drugs & alcohol | Medium | Exclude (or restrict) |
| Hate speech | High | Exclude |
| Military conflict | Medium | Exclude |
| Obscenity | High | Exclude |
| Spam/MFA | High | Exclude |
| Terrorism | High | Exclude |
| Sensitive social topics | Medium | Evaluate per brand |
| News — general | Low | Evaluate per brand |
| UGC — unmoderated | Medium | Exclude |

## Creative Size Requirements

### Standard Display Banners (IAB)

| Size | Name | Priority | Inventory |
|------|------|----------|-----------|
| 300x250 | Medium Rectangle | Must-have | Highest |
| 320x50 | Mobile Leaderboard | Must-have | Very high (mobile) |
| 728x90 | Leaderboard | Must-have | High (desktop) |
| 160x600 | Wide Skyscraper | Should-have | Medium |
| 300x600 | Half Page | Should-have | Medium |
| 336x280 | Large Rectangle | Nice-to-have | Medium |
| 970x250 | Billboard | Nice-to-have | Lower (premium) |
| 320x480 | Mobile Interstitial | Nice-to-have | Medium (mobile) |

**Minimum set:** 300x250 + 728x90 + 320x50 covers ~80% of available inventory.

### File Requirements

| Spec | Requirement |
|------|-------------|
| File size | <150KB (standard), <200KB (rich media) |
| File type | JPG, PNG, GIF, HTML5 |
| Animation | Max 30 seconds, must stop on last frame |
| Clickable area | Entire ad must be clickable |
| Border | 1px border required if background matches common site backgrounds |
