---
name: Landing Pages
domain: marketing
parent: marketing
tags: ["@cro", "@landing-pages", "@conversion", "@ab-testing"]
confidence: 0.38
---

# Landing Pages

Conversion rate optimization for performance marketing landing pages. Covers message match, page structure, A/B testing, friction analysis, form optimization, and page speed. The bridge between ad spend and conversions — a 10% improvement here multiplies the ROI of every upstream dollar.

This module focuses on landing pages for **paid traffic** — pages that receive visitors from ads and must convert them on a specific action. It complements the `writing` skill pack's copywriting module with performance-specific optimization.

## Content Structure

A performance landing page has a defined anatomy:

```
┌─────────────────────────────────┐
│ Hero (headline + subheadline +  │  ← Message match zone: must mirror the ad
│ CTA + hero image/video)         │
├─────────────────────────────────┤
│ Social proof bar                │  ← Logos, metrics, trust signals
├─────────────────────────────────┤
│ Problem / Pain                  │  ← Why the visitor needs this
├─────────────────────────────────┤
│ Solution / Benefits             │  ← What changes for them
├─────────────────────────────────┤
│ How it works (3 steps)          │  ← Reduce complexity anxiety
├─────────────────────────────────┤
│ Social proof (testimonials)     │  ← Proof the solution works
├─────────────────────────────────┤
│ Objection handling / FAQ        │  ← Remove remaining hesitation
├─────────────────────────────────┤
│ Final CTA                       │  ← Repeat the primary action
└─────────────────────────────────┘
```

Not every page needs every section. The principle: each section moves the visitor one step closer to the CTA.

## Key Concepts

### Message Match

The most common conversion killer in paid traffic. Message match = the ad promise matches the landing page delivery.

| Ad Element | Must Match On Page |
|------------|-------------------|
| **Headline** | Same benefit/outcome in the hero headline |
| **Offer** | Same offer (free trial, demo, discount, download) |
| **CTA** | Same action verb and next step |
| **Visual** | Same product/brand/imagery if ad used them |
| **Audience language** | Same tone and vocabulary |

**Message match impact (research-backed):**
- Dynamic Text Replacement matching ad keyword in headline: **+31% conversion lift** (Unbounce/ConversionLab, 77-day test, 1,274 visitors, 100% significance)
- Ad-to-page visual congruence (same image): **+48% conversion lift** (NextAfter, 239K sample)
- Full messaging continuity (headline + body + CTA aligned): **+63% conversion lift** (MarketingExperiments)
- Personalized landing pages convert **25% more** mobile users than static (Unbounce)

**Scoring message match:**
- **Strong** — Visitor instantly recognizes they're in the right place. Ad headline ≈ page headline. Visual continuity.
- **Moderate** — Same topic but different framing. Visitor has to read to confirm relevance.
- **Weak** — Generic page used for multiple ads. Visitor questions if they clicked the right thing.

Two principles (MarketingExperiments): **Continuity** (every step states the value prop consistently: ad → page → form → thank-you) and **Congruence** (every element on the page supports the same value prop).

**Common mismatch patterns:**
- **Offer mismatch** — Ad promises specific deal ("3 months free", "$12K value") but landing page shows generic brand messaging with no mention of the offer. Most damaging type.
- **Audience mismatch** — Ad targets enterprises but lands on a generic/personal page.
- **CTA mismatch** — Ad says "Get free trial" but page says "Contact sales."
- **Shared page for multiple ads** — One landing page serves many different ad variants. Diagnose: is DTR (dynamic text replacement) active? If not, the page can only match one ad's message.

Weak message match → high bounce rate → low Quality Score → higher CPC → worse everything.

### Friction Audit

Every element on the page either moves the visitor toward the CTA or creates friction. Audit for:

**Context matters:** These friction types apply to pages receiving paid traffic. For organic/SEO pages, full navigation and multiple paths are expected. Always ask: "Is this page receiving paid ad clicks?" before penalizing navigation or competing CTAs.

**Friction weight shifts by business model:**
- **PLG**: Navigation friction is #1 priority (every nav click leaks a paid visitor). Form friction is low (already 1-3 fields). Trust friction is low (free product, low commitment).
- **Sales-led B2B**: Trust friction is #1 priority (enterprise buyers need logos, security badges, compliance certs before submitting a demo form). Form friction is intentionally moderate (qualification fields are net positive for ACV >$5K). Navigation friction matters less (B2B buyers research extensively).
- **B2C ecommerce**: Speed friction is #1 priority (mobile shoppers bounce fastest). CTA friction is critical (one clear "Add to Cart"). Form friction matters at checkout (Baymard: 6-8 fields optimal).

| Friction Type | Examples | Fix |
|---------------|----------|-----|
| **Navigation friction** | Full site nav on paid landing page, too many links leaking paid clicks | Remove or minimize navigation on paid-traffic pages. One page, one action. Organic pages: nav is fine. |
| **Copy friction** | Vague headline, jargon, no clear benefit | Rewrite with clarity-first principle |
| **Visual friction** | Cluttered layout, unclear hierarchy, slow-loading images | Simplify, establish clear visual hierarchy |
| **Form friction** | Too many fields, unclear labels, no progress indicator | Reduce fields, add field labels, show progress |
| **Trust friction** | No social proof, no security badges, no clear privacy policy | Add proof elements, trust badges, privacy assurance |
| **CTA friction** | Vague button text, button below the fold, multiple competing CTAs | Clear action verb, above the fold, one primary CTA |
| **Speed friction** | Page loads in >3 seconds | Optimize images, reduce scripts, use CDN |

### A/B Test Prioritization

Two frameworks — use ICE for speed, PXL for objectivity.

**ICE Framework (fast, subjective):**

| Factor | Question | Score 1-10 |
|--------|----------|------------|
| **I**mpact | If this wins, how much will conversion rate improve? | 10 = transformative change |
| **C**onfidence | How confident am I this will win, based on data/research? | 10 = near-certain |
| **E**ase | How easy is this to implement and measure? | 10 = trivial |

ICE Score = (Impact + Confidence + Ease) / 3

**PXL Framework (objective, recommended for mature teams):**

Uses binary (true/false) questions with weighted points — eliminates subjective 1-10 scoring:

| Question | Points | Type |
|----------|--------|------|
| Is it above the fold? | 2 | Yes/No |
| Is the change noticeable within 5 seconds? | 2 | Yes/No |
| Does it add or remove an element (vs modify)? | 1 | Yes/No |
| Is it backed by user testing? | 1 | Yes/No |
| Is it backed by qualitative data (surveys, interviews)? | 1 | Yes/No |
| Is it backed by analytics/heatmap data? | 1 | Yes/No |
| Is it run on a high-traffic page? | 2 | Yes/No |
| Does it address a known conversion bottleneck? | 2 | Yes/No |

PXL avoids ICE's "Confidence" trap (where subjective confidence becomes circular logic).

**High-impact test categories (ranked by typical lift):**
1. Headline — Changing the core message. Highest potential impact.
2. CTA — Button text, placement, color, offer framing.
3. Social proof — Adding/changing testimonials, logos, metrics.
4. Form — Field count, layout, multi-step vs single-step.
5. Page length — Long-form vs short-form (depends on product complexity).
6. Visual — Hero image, product screenshot, video vs static.

### Statistical Significance

| Term | What It Means | Rule of Thumb |
|------|--------------|---------------|
| **Significance level** | Probability the result isn't random | Target 95% (p < 0.05) |
| **Power** | Probability of detecting a real effect | Target 80% |
| **Minimum Detectable Effect (MDE)** | Smallest improvement worth detecting | Depends on traffic volume |
| **Sample size** | Visitors needed per variation | Calculate before test, not after |
| **Test duration** | Minimum time to run | At least 1-2 full business cycles (7-14 days min) |

**When to call a test:**
- Both variations have reached minimum sample size
- Test has run for at least 7 days (captures day-of-week variance)
- Significance is ≥95% (or ≤5% for a clear loser)
- Don't peek daily and call early winners — this inflates false positive rate

### Form Optimization

Forms are the highest-friction element on most landing pages. Data-backed impact:

| Fields | Approx. CVR | Key Data |
|--------|------------|----------|
| 1-3 | ~50% | Sweet spot for simple lead gen (Crazy Egg) |
| 4-7 | ~34% | Plateau zone — rates level off (HubSpot) |
| 10+ | ~15-20% | Steep drop unless high motivation |

Reducing from 11 to 4 fields: **120%+ conversion increase** (Imagescape study). Reducing from 4 to 3: **~50% increase** (Crazy Egg).

**Assess before prescribing:** If form already has 1-3 fields, field reduction is NOT the lever. Pivot to post-signup activation (onboarding flow, time-to-value) and progressive profiling instead. Only recommend field reduction when current count is 5+.

| Principle | Application |
|-----------|-------------|
| **Multi-step > single long form** | Multi-step forms outperform single-step by 86% (Formstack). Breaking 10 fields into 3 steps reduces perceived effort. |
| **Progressive profiling** | Ask minimum on first visit (name, email). Enrich on subsequent visits (company, role, phone). Never re-ask known data. |
| **Smart defaults** | Pre-fill where possible (country from IP, company from email domain). |
| **Inline validation** | Show errors as the user types, not after submission. |
| **Progress indicator** | For multi-step forms, show "Step 2 of 3". |
| **Field labels above** | Labels above inputs outperform floating labels and placeholder-only patterns. |
| **CTA button text** | "Get my free report" > "Submit". Describe what they get, not what they do. |

### Page Speed

| Metric | Target | Why |
|--------|--------|-----|
| **Largest Contentful Paint (LCP)** | <2.5s | Measures when main content loads |
| **Interaction to Next Paint (INP)** | <200ms | Measures interactivity (replaced FID in Mar 2024) |
| **Cumulative Layout Shift (CLS)** | <0.1 | Measures visual stability |
| **Total page load** | <3s | 53% of mobile visitors leave if >3s (Google) |

**Per-second conversion impact (Portent study, 100M+ page views):**

| Load Time | E-Commerce CVR | B2B Lead Gen CVR |
|-----------|---------------|-----------------|
| 1 second | 3.05% | ~40% |
| 2 seconds | 1.68% | ~34% |
| 3 seconds | 1.12% | ~29% |
| 5 seconds | ~0.50% | ~20% |

0-5 seconds: CVR drops **4.42% per additional second**. 1s sites convert at 2.5-3x the rate of 5s sites.

Speed optimization checklist:
- Compress and lazy-load images
- Minimize JavaScript (especially third-party scripts)
- Use a CDN
- Enable browser caching
- Defer non-critical scripts
- Use modern image formats (WebP, AVIF)

## Inputs & Outputs

**Inputs:**
- Ad copy and creative (for message match analysis)
- Current landing page URL
- Conversion data (traffic volume, conversion rate, bounce rate)
- Business goal and target CPA/ROAS (from `media-context.md`)
- Audience description

**Outputs:**
- Message match audit (score + specific mismatches)
- Friction audit (prioritized list of friction points)
- CRO recommendations (prioritized by ICE score)
- A/B test plan (hypothesis, variations, sample size, duration)
- Page copy recommendations (headline, CTA, sections)
- Technical optimization checklist (speed, mobile, tracking)

## Modes

| Mode | What You're Doing |
|------|-------------------|
| **Audit** | Full page review — message match, friction, speed, mobile |
| **Optimize** | Implementing specific improvements based on audit findings |
| **Test** | Designing, launching, and analyzing A/B tests |
| **Build** | Creating new landing page spec from ad campaign brief |
| **Report** | Analyzing conversion data, test results, page performance |

## Common Tasks

1. **Landing page audit** — Full conversion review:
   - Score message match against running ads (strong/moderate/weak)
   - Run friction audit across all 7 friction types
   - Check page speed (Core Web Vitals)
   - Review mobile experience (thumb-friendly CTA, readable text, no horizontal scroll)
   - Verify tracking (conversion events fire, UTMs preserved)
   - Prioritize findings by ICE score
   - Deliver action items with expected impact

2. **Design A/B test** — Plan a structured test:
   - State hypothesis: "Changing [element] from [current] to [proposed] will increase [metric] by [expected %] because [reason]"
   - Define primary metric (conversion rate, not bounce rate)
   - Calculate required sample size for 95% significance
   - Estimate test duration based on current traffic
   - Design control and variant (one change only)
   - Define stopping rules (when to call it)

3. **Message match optimization** — Align ad-to-page:
   - Map each ad group's headlines to landing page headlines
   - Identify mismatches (different benefit, different offer, different CTA)
   - Write page headline variants that match ad copy
   - Recommend one landing page per ad group theme (or dynamic text replacement)
   - Verify offer and CTA consistency

4. **Form optimization** — Reduce form friction:
   - Count current fields, classify as essential vs nice-to-have
   - Recommend field reduction (target: 3-5 fields for lead gen)
   - Propose multi-step if >5 fields are required
   - Review field labels, error handling, CTA text
   - Estimate conversion lift from reduction

5. **Page speed optimization** — Technical performance:
   - Run Core Web Vitals assessment
   - Identify largest performance bottlenecks
   - Prioritize fixes by impact and ease
   - Provide specific technical recommendations
   - Set target metrics

## Tips

- The landing page is not your homepage. Paid traffic landing pages have one goal. Remove navigation, sidebars, and competing CTAs.
- Message match is the easiest win. Matching the ad headline to the page headline can lift conversion rates 20-50% with minimal effort.
- Mobile-first is non-negotiable. 60%+ of paid social traffic is mobile. If the page doesn't work perfectly on a phone, you're burning ad spend.
- Social proof above the fold. Logo bars and trust signals visible without scrolling reduce bounce rate immediately.
- Test big changes first. A completely different headline or page structure will give you a clearer signal than testing button colors.
- One page per audience. If you're running ads to marketers and developers, they need different landing pages, not one page that tries to speak to both.

## Gotchas

- **Testing too many things** — Multivariate tests need exponentially more traffic. With <10K visitors/month, stick to A/B (two variations) and test one element.
- **Calling tests too early** — Checking results after 2 days and declaring a winner is statistical gambling. Wait for minimum sample size and 7+ days.
- **Ignoring post-click experience** — The page might convert, but if the thank-you page is broken or the email sequence doesn't trigger, the lead is wasted. Test the full post-conversion flow.
- **Dynamic text replacement gone wrong** — DTR (inserting the search keyword into the page headline) works for exact match keywords but creates nonsense with broad match. "Best crm software for small business" as a headline looks like SEO spam.
- **Page speed regression** — Adding tracking scripts, chat widgets, and heatmap tools slows the page. Monitor speed after every change. A page that was fast at launch is often slow 6 months later.
- **Optimizing for micro-conversions** — Testing for "scroll depth" or "button hover" instead of actual conversions produces meaningless wins. Always test against the real conversion event.

## References

- `references/cro-checklist.md` — complete CRO audit checklist with scoring, test templates, page structure specs

## Related Modules

- **paid-search** — Quality Score depends on landing page experience
- **paid-social** — ad-to-page consistency for social campaigns
- **analytics** — conversion tracking setup, test result analysis
