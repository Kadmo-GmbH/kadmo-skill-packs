---
name: Paid Search
domain: marketing
parent: marketing
tags: ["@ppc", "@google-ads", "@bing-ads", "@search"]
confidence: 0.38
---

# Paid Search

Campaign management for Google Ads and Microsoft Ads search campaigns. Covers campaign structure, keyword strategy, match types, ad copy (RSA), Quality Score, bidding strategies, negative keywords, and ongoing optimization. Designed for agents that build, audit, and optimize search campaigns autonomously.

This module is **account-agnostic**. Account-specific details (budgets, products, audiences) come from the `media-context.md` file provided at runtime.

## Content Structure

Search campaigns follow a strict hierarchy:

```
Account
└── Campaign (budget, location, bidding strategy)
    └── Ad Group (theme, keywords, ads)
        ├── Keywords (with match types)
        ├── Responsive Search Ads (RSAs)
        └── Ad Extensions (sitelinks, callouts, structured snippets)
```

Each level has one job:
- **Campaign** = budget container + targeting scope (geo, device, schedule)
- **Ad Group** = theme container. One intent per ad group.
- **Keywords** = the queries you want to match
- **Ads** = the message shown for those queries

## Key Concepts

### Campaign Types

| Type | When to Use | Bidding | Key Consideration |
|------|-------------|---------|-------------------|
| **Search** | Capturing existing demand | tCPA, tROAS, Max Conv. | Keyword control, negative management |
| **Performance Max** | Broad coverage, ecommerce | tROAS, Max Conv. Value | Limited visibility, asset group structure |
| **Shopping** | Product feeds, ecommerce | tROAS, Manual CPC | Feed quality is everything |
| **Dynamic Search Ads** | Coverage gaps, large catalogs | tCPA | Needs strong negative keyword lists |

### Match Types (Current Behavior)

| Match Type | Syntax | Reaches | Use Case |
|------------|--------|---------|----------|
| **Exact** | [keyword] | Close variants, same intent | High-intent, proven converters |
| **Phrase** | "keyword" | Contains the meaning | Mid-funnel, controlled expansion |
| **Broad** | keyword | Related intent, synonyms, context | Discovery, smart bidding required |

Match type strategy: Start with exact + phrase for proven terms. Use broad only with smart bidding and sufficient conversion data (30+ conversions/month at campaign level).

### Quality Score

Three components, weighted approximately:

| Component | Weight | What It Measures | How to Improve |
|-----------|--------|------------------|----------------|
| **Expected CTR** | ~39% | Will people click your ad? | Write compelling ad copy, use ad extensions, improve ad rank |
| **Ad Relevance** | ~22% | Does the ad match the query intent? | Tight ad group themes, keywords in headlines, match intent |
| **Landing Page Experience** | ~39% | Does the page deliver on the ad promise? | Message match, page speed, mobile UX, relevant content |

Quality Score impacts: CPC (higher QS = lower CPC), ad rank, eligibility for extensions, top-of-page thresholds.

### Bidding Strategy Decision Framework

| Scenario | Recommended Strategy | Prerequisites |
|----------|---------------------|---------------|
| New campaign, no conversion data | Maximize Clicks (capped) | Set max CPC cap to control costs |
| 15-30 conversions/month | Maximize Conversions | Conversion tracking verified |
| 30+ conversions/month, known CPA target | Target CPA | Stable CPA history, realistic target |
| Ecommerce, 30+ conv/month | Target ROAS | Revenue tracking, sufficient data |
| Brand campaigns, impression share goal | Target Impression Share | Brand terms only, set max CPC cap |
| Manual control needed | Manual CPC (enhanced optional) | Experienced operator, low volume |

Learning phase: After any bidding change, allow 1-2 weeks (or 50 conversions) for the algorithm to stabilize. Do not judge performance during learning.

**PLG/freemium caveat:** If the conversion event is a free signup (zero friction), platforms will optimize for signup volume — which may not correlate with paid conversion. For PLG products, import downstream conversion events (trial-to-paid, activation milestones) as offline conversions and optimize toward those instead. Without this, smart bidding will flood you with low-quality free signups.

**B2B caveat:** For high-ACV B2B (demo requests → sales cycle → close), platform-reported CPA only measures the demo request, not the closed deal. Import offline CRM conversions (demo → opportunity → closed-won) to give smart bidding accurate signals about which clicks actually generate revenue.

### Naming Convention

Use structured naming for easy filtering and reporting:

```
Campaign:  {brand}_{objective}_{channel}_{geo}_{targeting}
           acme_leads_search_us_nonbrand
           acme_sales_shopping_de_allproducts

Ad Group:  {theme}_{match-type}
           crm-software_exact
           crm-software_phrase

UTM:       utm_source=google&utm_medium=cpc&utm_campaign={campaign}&utm_content={adgroup}&utm_term={keyword}
```

### RSA Testing Methodology

RSAs can't be A/B tested traditionally (43,680 possible combinations from 15 headlines + 4 descriptions). Use these structured approaches instead.

**Method 1: Ad Variations (single-variable isolation)**
- Use Google Ads Experiments > Ad Variations > Find and Replace
- Swap one element across all RSAs (e.g., all "Free Trial" CTAs → "Get Started")
- Run 50/50 split for 2-4 weeks minimum
- Google evaluates at 95% confidence using Jackknife resampling
- Best for: testing specific copy elements (CTA, USP, pricing framing)

**Method 2: Theme-based messaging tests**
- Create separate RSAs, each carrying one coherent messaging theme
- Themes: ROI (cost savings), Risk (security/compliance), Speed (deployment), Proof (reviews/logos), Fit (audience-specific)
- Never blend themes within one RSA — keep each thematically pure
- Run as Drafts & Experiments with 50% traffic split, 4-8 weeks

**Asset performance labels (Google's rating system):**

| Label | Meaning | Threshold | Action |
|-------|---------|-----------|--------|
| **Learning** | Still evaluating | <500 impressions on asset | Wait |
| **Low** | Underperforms vs peers in ad group | 500+ impressions | Replace with new asset |
| **Good** | Adequate performance | 500+ impressions | Keep as benchmark |
| **Best** | Top performer in category | 500+ impressions | Protect; create similar |

Labels require 2,000+ impressions in "Google Search: Top" over 30 days for reliability. Labels compare within ad group only, not across campaigns. Google optimizes primarily for CTR, which may not align with conversion goals.

**Asset count guidance:** 8-10 headlines and 3 descriptions outperform filling all 15 headline slots in smaller campaigns lacking impression volume to test all combinations (Optmyzr study, 93K RSAs).

**Minimum data for testing decisions:**

| Expected Lift | Conversions Per Variant | Practical Timeline (40 conv/mo) |
|--------------|------------------------|--------------------------------|
| 50% | ~100 | ~5 weeks |
| 30% | ~250 | ~12 weeks |
| 20% | ~400 | ~20 weeks |

Accounts with <100 monthly conversions can only detect 30%+ differences reliably. Test big changes (themes, CTAs) not subtle tweaks.

**Testing cadence:**
- Weekly: review asset labels, replace "Low" assets, flag zero-impression assets >14 days
- Monthly: run one documented theme test per high-priority campaign
- Quarterly: full asset library audit, retire stale themes, introduce new angles

## Inputs & Outputs

**Inputs:**
- Business goal and KPI targets (from `media-context.md`)
- Product/service descriptions and USPs
- Target audience and geos
- Budget constraints
- Existing keyword lists and historical data (if available)
- Landing page URLs

**Outputs:**
- Campaign structure document
- Keyword lists with match types and initial bids
- RSA headlines (15) and descriptions (4) per ad group
- Negative keyword lists (campaign and account level)
- Extension copy (sitelinks, callouts, structured snippets)
- Bidding strategy recommendation
- UTM tracking schema

## Modes

| Mode | What You're Doing | Key Actions |
|------|-------------------|-------------|
| **Build** | Creating new campaigns from scratch | Structure, keyword research, ad copy, extensions, tracking |
| **Audit** | Reviewing existing campaign health | Structure review, wasted spend, QS analysis, gap analysis |
| **Optimize** | Improving live campaign performance | Bid adjustments, negative keywords, ad testing, budget reallocation |
| **Scale** | Expanding successful campaigns | New keywords, geo expansion, match type broadening, budget increase |
| **Report** | Communicating performance | KPI vs target, trend analysis, actionable recommendations |

## Common Tasks

1. **Build search campaign** — Design full campaign structure from product brief:
   - Map product/service → campaign themes
   - Research keywords per theme (branded, non-branded, competitor)
   - Group keywords into tight ad groups (10-20 keywords per group)
   - Write RSAs per ad group (see RSA framework below)
   - Create sitelinks, callouts, structured snippets
   - Set bidding strategy based on data availability
   - Define negative keyword lists
   - Configure conversion tracking and UTMs

2. **Write RSA copy** — Create Responsive Search Ads:
   - Write 15 headlines (30 char limit each):
     - 3-4 with primary keyword/theme
     - 2-3 with key benefit/USP
     - 2-3 with social proof (numbers, awards)
     - 2 with CTA
     - 2 with offer/pricing
     - 1-2 with brand name
     - 1-2 dynamic (keyword insertion, countdown, location)
   - Write 4 descriptions (90 char limit each):
     - 1 benefit-focused with CTA
     - 1 feature-focused with specifics
     - 1 social proof / trust signal
     - 1 offer detail / urgency
   - Pin only when necessary (pin headline 1 and 2 if brand compliance requires it; unpinned = better optimization)

3. **Keyword research** — Build keyword universe (structured 5-step process):
   - **Plant:** Generate 10-20 seed keywords in three categories: product-based (what you sell), problem-based (what customers struggle with), solution-based (what outcome they want)
   - **Expand:** Use keyword tools, competitor analysis, search term reports, and "People Also Ask" mining to build universe. Target 200-2,000 keywords depending on market size.
   - **Classify intent:** Score each keyword: Transactional (buy/price/cost/near me → highest value), Commercial investigation (best/review/comparison → mid value), Informational (how to/what is → low PPC value, consider excluding)
   - **Prioritize:** Score keywords using Keyword Opportunity Score: `KOS = (Volume × Intent Score × Est. CVR) / (Competition × Est. CPC)`. Focus on top quartile.
   - **Group:** Cluster by theme into ad groups (10-20 keywords per group). Each cluster = one intent = one ad group.
   - **Assign match types:** Exact for proven high-intent, Phrase for controlled expansion, Broad only with smart bidding + 30+ conversions/month
   - See `references/keyword-research.md` for full methodology with scoring rubric

4. **Search term analysis** — Review and classify using the Four-Bucket Model:
   - Export search terms report (last 30 days minimum, 90 days for trends)
   - **Bucket 1 — High-Intent Converters:** Terms with conversions or strong commercial signals → promote to keyword in relevant ad group
   - **Bucket 2 — Promising Prospects:** Decent CTR, no conversions yet → monitor 30-60 days, don't negate prematurely
   - **Bucket 3 — Irrelevant Junk:** Informational (how-to, free, jobs), geographic mismatches, unrelated products → add as negative
   - **Bucket 4 — Brand Terms:** Company/product name queries → route to dedicated brand campaign
   - **Decision thresholds:** Cost > 2x target CPA with 0 conversions → add negative. CTR < 1% with 100+ impressions → review relevance. Clicks > 60 with 0 conversions → strong negative candidate.
   - First pass typically finds 40-60% irrelevant terms; expect 30-50% waste reduction in week one
   - Run n-gram analysis on top 500 search terms to find recurring patterns (e.g., "free" appears in 8% of queries → campaign-level negative)

5. **Campaign audit** — Assess campaign health:
   - Structure: Are ad groups tightly themed?
   - Quality Score: Average QS, distribution, low-QS terms
   - Wasted spend: Non-converting keywords, poor search terms
   - Coverage: Impression share, lost IS (budget vs rank)
   - Ad copy: RSA asset performance ratings (Low/Good/Best)
   - Extensions: Are all relevant extensions active?
   - Bidding: Is strategy appropriate for data volume?
   - Tracking: Are conversions firing correctly?

6. **Bid optimization** — Adjust bids/targets:
   - Review CPA/ROAS by campaign, ad group, keyword
   - Identify over- and under-performing segments
   - Adjust targets incrementally (10-15% at a time)
   - Check device, geo, schedule, audience bid adjustments
   - Allow learning phase after changes

## Tips

- Start narrow, expand later. It's easier to scale a profitable structure than to fix a bloated one.
- Ad group themes beat keyword count. 5 tightly themed keywords outperform 50 loosely related ones.
- Negative keywords are as important as positive keywords. Review search terms weekly for new campaigns, biweekly for mature ones.
- RSA headline diversity matters. Google needs different combinations to test. 15 headlines that all say the same thing differently won't help.
- Don't chase Quality Score as a KPI. Fix it when it's dragging costs up, but optimize for conversions first.
- Impression share tells you headroom. If you're at 80% IS with good CPA, there's room to scale with more budget.
- Smart bidding needs data. Don't enable tCPA on a campaign with 5 conversions/month. It will thrash.
- Brand campaigns are cheap insurance. Even if you rank #1 organically, competitors can bid on your brand terms.

## Gotchas

- **Broad match without smart bidding** — Broad match relies on Google's intent matching, which requires conversion signals. Without smart bidding, broad match burns budget on irrelevant queries.
- **Too many ad groups** — Splitting keywords into single-keyword ad groups (SKAGs) was best practice in 2018. With RSAs and smart bidding, themed ad groups of 10-20 keywords perform better and are manageable.
- **Ignoring ad strength** — Google's "Ad Strength" metric for RSAs isn't a performance predictor, but "Poor" ad strength can limit serving. Aim for "Good" minimum, don't obsess over "Excellent."
- **Changing bids during learning** — Smart bidding needs 1-2 weeks to calibrate. Adjusting targets during learning phase resets the cycle and creates volatility.
- **Shared budgets masking problems** — Shared budgets across campaigns prevent proper pacing analysis. Use campaign-level budgets unless campaigns have identical priority.
- **Close variant matching** — Exact match now includes close variants, synonyms, and implied intent. An exact match [running shoes] may match "jogging sneakers." Monitor and add negatives.
- **Performance Max cannibalization** — PMax campaigns can compete with your search campaigns for the same queries. Use brand exclusions and campaign-level negative keywords to manage overlap.
- **Conversion tracking gaps** — If offline conversions matter (calls, in-store visits), platform-reported CPA is misleading. Import offline conversion data or adjust targets to compensate.

## References

- `references/campaign-structures.md` — campaign templates for common business types (ecommerce, SaaS, lead gen, local), STAG/Hagakure frameworks, RSA headline templates
- `references/keyword-research.md` — keyword universe building (5-step), intent classification, KOS scoring, negative keyword scoring (0-100), n-gram analysis, decision rules

## Related Modules

- **analytics** — measurement setup, attribution, and reporting for search campaigns
- **landing-pages** — landing page optimization for Quality Score and conversion rate
- **paid-social** — cross-channel audience insights (search query data informs social targeting)
