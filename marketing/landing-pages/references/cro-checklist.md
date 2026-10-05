# CRO Audit Checklist

Complete conversion rate optimization checklist for performance marketing landing pages. Score each item as Pass / Fail / N/A. Calculate completion rate to prioritize improvements.

## 1. Message Match (Weight: Critical)

- [ ] Page headline reflects the ad headline's promise
- [ ] Page offer matches the ad offer (same free trial, demo, discount)
- [ ] CTA button text matches ad CTA
- [ ] Visual consistency (same product image, brand colors)
- [ ] Audience language matches between ad and page
- [ ] One dedicated page per ad group theme (or DTR configured)

**Score: __ / 6**

## 2. Above the Fold (Weight: Critical)

- [ ] Headline communicates primary benefit in under 10 words
- [ ] Subheadline adds specificity (how, for whom, or proof)
- [ ] Primary CTA visible without scrolling (desktop and mobile)
- [ ] CTA button text describes what they GET, not what they DO
- [ ] Hero image/video supports the message (not generic stock)
- [ ] No competing links or navigation that could leak traffic
- [ ] Social proof element visible (logos, metric, trust badge)

**Score: __ / 7**

## 3. Copy Quality (Weight: High)

- [ ] Benefits lead, features support (not feature-first)
- [ ] Specific claims, not vague promises ("40% faster" not "significantly faster")
- [ ] Customer language used (not internal jargon)
- [ ] One idea per section
- [ ] Every claim has supporting proof (metric, quote, logo)
- [ ] CTA repeated at logical intervals (hero, mid-page, bottom)
- [ ] Scannable: subheadings every 200-300 words, bullet points for lists
- [ ] No AI-sounding phrases ("in today's landscape", "leverage", "comprehensive solution")

**Score: __ / 8**

## 4. Social Proof (Weight: High)

- [ ] At least one social proof element on the page
- [ ] Social proof is specific and attributed (name, title, company)
- [ ] Proof matches the audience (testimonials from ICP, not random users)
- [ ] Proof addresses a likely objection
- [ ] Metrics are specific ("2,400 teams" not "thousands of users")
- [ ] Logos section includes recognizable names (if available)

**Score: __ / 6**

## 5. Form & Conversion Flow (Weight: High)

**Field count targets vary by business model:**
- PLG/B2C: 1-3 fields (email + minimal). If already at 1-3, pivot to post-signup activation.
- Sales-led B2B (ACV >$5K): 4-6 fields acceptable. Qualification fields (company size, role) are intentional friction that improves lead quality.
- B2C ecommerce: Checkout fields per Baymard (6-8 optimal for guest checkout).

- [ ] Form field count is appropriate for business model (see targets above)
- [ ] Every field is necessary — for PLG: the immediate next step; for B2B: sales routing and qualification
- [ ] Field labels are above inputs (not placeholder-only)
- [ ] Required fields are marked
- [ ] Error messages are specific and inline
- [ ] CTA button is descriptive ("Get my free report" not "Submit")
- [ ] Multi-step form used if >5 fields required (with progress indicator)
- [ ] Thank you page / confirmation is meaningful (not just "Thanks!")
- [ ] Post-conversion flow works (email triggers, redirect, CRM entry)

**Score: __ / 9**

## 6. Trust & Objection Handling (Weight: Medium)

- [ ] FAQ section addresses top 3-5 objections
- [ ] Privacy assurance near form ("We won't spam you" or privacy link)
- [ ] Security badges displayed if collecting payment/sensitive info
- [ ] Money-back guarantee or free trial mentioned (if applicable)
- [ ] Contact alternative available (phone, chat) for hesitant visitors
- [ ] Real company details visible (not anonymous landing page)

**Score: __ / 6**

## 7. Mobile Experience (Weight: Critical)

- [ ] Page renders correctly on 320px width (smallest common)
- [ ] CTA button is thumb-friendly (min 44x44px tap target)
- [ ] Text is readable without zooming (16px minimum body text)
- [ ] No horizontal scrolling
- [ ] Form fields are mobile-optimized (correct keyboard types)
- [ ] Images don't push content below the fold on mobile
- [ ] Click-to-call enabled for phone numbers

**Score: __ / 7**

## 8. Page Speed (Weight: High)

- [ ] LCP (Largest Contentful Paint) < 2.5 seconds
- [ ] INP (Interaction to Next Paint) < 200ms
- [ ] CLS (Cumulative Layout Shift) < 0.1
- [ ] Total page weight < 3MB
- [ ] Images are compressed and lazy-loaded
- [ ] No render-blocking JavaScript above the fold
- [ ] CDN configured for static assets

**Score: __ / 7**

## 9. Tracking & Analytics (Weight: High)

- [ ] Conversion event fires on form submit / purchase (verified in tag manager)
- [ ] UTM parameters preserved through to conversion
- [ ] Google Analytics (GA4) event configured
- [ ] Ad platform pixel fires (Google, Meta, LinkedIn, TikTok — whichever is active)
- [ ] Server-side tracking configured (if available)
- [ ] Thank you page / conversion confirmation trackable
- [ ] No duplicate conversion fires
- [ ] Cross-domain tracking configured (if applicable)

**Score: __ / 8**

## 10. Navigation & Focus (Weight: Medium)

- [ ] Main site navigation removed or minimized
- [ ] No sidebar or unrelated content
- [ ] One primary CTA (secondary allowed if lower visual weight)
- [ ] No external links that leak traffic (open in new tab if necessary)
- [ ] Footer is minimal (legal links only, no full site footer)
- [ ] Back button works (doesn't break or redirect)

**Score: __ / 6**

---

## Scoring Summary

| Section | Max | Score | Priority |
|---------|-----|-------|----------|
| 1. Message Match | 6 | __ | Critical |
| 2. Above the Fold | 7 | __ | Critical |
| 3. Copy Quality | 8 | __ | High |
| 4. Social Proof | 6 | __ | High |
| 5. Form & Conversion | 9 | __ | High |
| 6. Trust & Objections | 6 | __ | Medium |
| 7. Mobile Experience | 7 | __ | Critical |
| 8. Page Speed | 7 | __ | High |
| 9. Tracking | 8 | __ | High |
| 10. Navigation & Focus | 6 | __ | Medium |
| **TOTAL** | **70** | **__** | |

**Scoring guide:**
- 60-70: Production-ready
- 45-59: Needs optimization (address Critical items first)
- 30-44: Significant issues (audit before increasing ad spend)
- <30: Do not send paid traffic until major issues resolved

---

## A/B Test Template

### Test Design

| Field | Value |
|-------|-------|
| **Test name** | |
| **Hypothesis** | Changing [element] from [current] to [proposed] will [increase/decrease] [metric] by [expected %] because [reason] |
| **Primary metric** | Conversion rate (form submit / landing page view) |
| **Secondary metrics** | Bounce rate, scroll depth, time on page |
| **Variant A (control)** | [Current page] |
| **Variant B** | [Change description] |
| **Traffic split** | 50/50 |
| **Minimum sample** | [Calculate based on baseline CVR and MDE] |
| **Test duration** | [Minimum 7 days, calculate from daily traffic] |
| **Significance target** | 95% |

### Sample Size Calculator

For a baseline conversion rate of X% and minimum detectable effect of Y%:

| Baseline CVR | MDE (relative) | Sample per variation |
|-------------|-----------------|---------------------|
| 1% | 20% (to 1.2%) | 131,000 |
| 2% | 20% (to 2.4%) | 64,000 |
| 3% | 20% (to 3.6%) | 41,600 |
| 5% | 20% (to 6.0%) | 24,000 |
| 10% | 20% (to 12.0%) | 11,200 |
| 2% | 50% (to 3.0%) | 11,500 |
| 5% | 50% (to 7.5%) | 4,200 |

Rule of thumb: If your page gets <1,000 visitors/week, only test changes expected to produce >50% relative lift. Small improvements need too much traffic to detect.

### Post-Test Analysis Template

| Metric | Control | Variant | Difference | Significance |
|--------|---------|---------|------------|-------------|
| Conversion rate | | | | |
| Bounce rate | | | | |
| Avg time on page | | | | |
| Scroll to CTA | | | | |

**Decision:** [ Implement variant / Keep control / Inconclusive — need more data ]

**Learnings:** [What did this test teach us about the audience?]

**Next test:** [Based on this result, what should we test next?]
