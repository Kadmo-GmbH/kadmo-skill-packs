---
name: Email Sequences
domain: marketing
parent: marketing
tags: ["@email", "@sequences", "@nurture", "@drip"]
confidence: 0.42
---

# Email Sequences

Performance marketing email sequences — automated emails triggered by ad-driven conversions, form submissions, and behavioral events. Covers welcome sequences, abandoned cart recovery, lead nurture, post-purchase, and re-engagement. Scoped to **performance email only** (sequences that directly support paid acquisition ROI), not general CRM/lifecycle email.

This module is **platform-agnostic**. It works with any ESP (Klaviyo, HubSpot, Mailchimp, ActiveCampaign, Customer.io). Account-specific details come from `media-context.md`.

## Content Structure

Email sequences follow a trigger → sequence → branch architecture:

```
Trigger Event (form submit, purchase, cart abandon, etc.)
└── Sequence (ordered emails with timing delays)
    ├── Email 1 (immediate or short delay)
    ├── Wait (hours/days)
    ├── Branch (did they open? click? convert?)
    │   ├── Yes → next sequence or exit
    │   └── No → alternative path or reminder
    ├── Email 2
    └── ... (3-7 emails typical)
```

## Key Concepts

### Sequence Types

| Type | Trigger | Goal | Typical Length | Timing |
|------|---------|------|---------------|--------|
| **Welcome (sales-led)** | Form submit, signup | Build trust, educate, drive demo/trial | 3-5 emails | Day 0, 1, 3, 5, 7 |
| **Welcome (PLG)** | Product signup | Activate user (first value moment), invite team, upgrade | 3-5 emails | Day 0, 1, 3, 5, 7 |
| **Abandoned cart** | Cart add without purchase | Recover purchase | 3 emails | 1h, 24h, 72h |
| **Lead nurture** | Lead form submit (B2B) | Educate, qualify, move to sales | 5-7 emails | Day 0, 2, 5, 8, 12, 16, 21 |
| **Post-purchase** | Purchase completed | Onboard, cross-sell, review | 4-5 emails | Day 0, 3, 7, 14, 30 |
| **Re-engagement** | 60-90 days inactive | Win back or clean list | 3 emails | Day 0, 5, 10 |
| **Browse abandonment** | Product view without cart | Nudge toward purchase | 2 emails | 4h, 24h |

**Cadence by business model:**

| Business Model | Max Frequency | Interval Between Emails | Total Sequence Length |
|---------------|--------------|------------------------|---------------------|
| PLG (Notion, Slack) | Daily first week, then 2x/week | 1-3 days | 7-14 days |
| Sales-led B2B (Deel) | 2x/week max during nurture | 3-5 days | 21-60 days |
| B2C ecommerce | Daily for cart recovery, 2-3x/week otherwise | 1-4 days | 7-30 days |
| B2B enterprise (long cycle) | Weekly | 5-7 days | 30-90 days |

PLG sequences optimize for activation speed (get user to value fast). B2B sequences optimize for qualification depth (educate over weeks). Applying B2C cadence to B2B causes unsubscribes.

### Email Copy Framework

**Subject lines (28-39 characters optimal for mobile):**
- Personalized: +26% open rate (Experian). Formula: `[Name], [curiosity hook]`
- Numbers: +15-20% open rate. Formula: `[Number] [adjective] [noun] [promise]`
- Questions: +10-15% open rate (B2B). Formula: `Are you [making this mistake]?`
- Urgency: +22% open rate, but fatigues after 3 sends. Use sparingly.

**Preheader:** 40-100 characters. Complement the subject line, don't repeat it. Adding a preheader lifts open rates ~7% (Litmus).

**Body (optimal length by type):**

| Type | Words | Key Rule |
|------|-------|----------|
| Welcome | 100-200 | One clear next step |
| Promotional | 50-150 | Single CTA |
| Newsletter | 200-500 | Value-dense content |
| Re-engagement | 25-75 | Ultra-short, direct |
| Transactional | <100 | Task completion focus |

**Critical rule:** Single CTA per email. Multiple CTAs = +371% fewer clicks (WordStream). Every email has one job.

### Segmentation Strategy

Segment by acquisition source and behavior, not just demographics:

| Segment Dimension | How | Why |
|-------------------|-----|-----|
| **Acquisition source** | UTM source/medium from paid campaign | Different channels = different intent levels |
| **Funnel stage** | Form type, page visited, content consumed | Tailor message to awareness level |
| **Engagement level** | Opens, clicks, site visits in last 30 days | Active vs dormant = different cadence |
| **Purchase behavior** | First purchase, repeat, high-value (RFM) | Cross-sell vs upsell vs loyalty |
| **Lead score** | Engagement + fit scoring (if available) | Prioritize sales-ready leads |

### Deliverability Checklist

Technical requirements before sending any sequence:

| Check | What | Status |
|-------|------|--------|
| **SPF** | DNS TXT record authorizing your sending IP | Required |
| **DKIM** | DNS TXT record for email signature verification | Required |
| **DMARC** | DNS TXT record specifying authentication policy | Required |
| **Warm-up** | Gradually increase send volume over 2-4 weeks for new domains/IPs | Required for new senders |
| **List hygiene** | Remove bounces, unsubscribes, and inactive (6mo+) before every campaign | Required |
| **Double opt-in** | Confirm subscription via email link | Recommended (required in some regions) |
| **Unsubscribe link** | One-click unsubscribe in email header (RFC 8058) | Required by law |

**Warm-up schedule (new domain/IP, Infobip methodology):**

| Day | Volume | Target Audience |
|-----|--------|----------------|
| 1-3 | 500-1,000 | Most engaged (opened/clicked last 30d) |
| 4-7 | 1,400-4,000 | Engaged last 30 days |
| 8-10 | 5,600-11,200 | Broaden to last 60 days |
| 11-14 | 16,000-50,000 | Active last 60 days |
| Week 3-4+ | Increase 40%/day | Exclude 90+ day inactive |

After 14 days, increase 40% daily based on previous day's delivered volume. First 45 days: exclude users inactive 90+ days. Monitor bounce rate (<2%), spam complaint rate (<0.1%), and inbox placement throughout.

### A/B Testing

**Test priority (highest impact first):**
1. Subject line (10-40% open rate variance)
2. From name/address (10-30% variance)
3. Send time/day (5-20% variance)
4. CTA copy + placement (10-30% CTR variance)
5. Email length/format (5-20% CTR variance)

**Minimum sample sizes (95% confidence, 80% power):**

| Baseline | MDE | Min Per Variant |
|----------|-----|----------------|
| 20% open rate | 5pp lift | ~700 |
| 3% CTR | 1pp lift | ~3,600 |
| 1% conversion | 0.5pp lift | ~5,200 |

Rule of thumb: minimum 1,000 per variant for subject line tests, 5,000 for CTR tests. Run for at least 24 hours to capture timezone variation.

### Benchmarks by Email Type

| Type | Open Rate | CTR | Conversion Rate |
|------|-----------|-----|----------------|
| Welcome | 50-60% | 8-15% | Highest (first impression) |
| Abandoned cart | 40-50% | 5-10% | 5-10% recovery rate |
| Lead nurture | 20-30% | 2-5% | Varies by offer |
| Post-purchase | 40-50% | 5-8% | Cross-sell dependent |
| Re-engagement | 10-20% | 1-3% | List cleaning purpose |
| Promotional | 15-25% | 2-4% | Industry average |

## Inputs & Outputs

**Inputs:**
- Trigger event definitions (from ad campaigns, forms, ecommerce events)
- Audience segments (from `media-context.md`)
- Product/offer details
- Brand voice guidelines
- ESP platform and capabilities

**Outputs:**
- Sequence architecture (trigger → emails → branches → timing)
- Email copy per sequence (subject, preheader, body, CTA)
- Segmentation rules
- A/B test plan for sequence optimization
- Performance monitoring setup (open rate, CTR, conversion, unsubscribe)

## Modes

| Mode | What You're Doing |
|------|-------------------|
| **Build** | Creating new sequences from scratch |
| **Audit** | Reviewing existing sequence performance, deliverability health |
| **Optimize** | A/B testing subject lines, timing, copy; improving conversion |
| **Expand** | Adding new sequences for additional triggers or segments |

## Common Tasks

1. **Build welcome sequence** — 3-5 emails triggered by form submission:
   - Email 1 (immediate): Deliver promised asset + set expectations
   - Email 2 (Day 1): Introduce brand/value, single helpful resource
   - Email 3 (Day 3): Social proof (testimonial, case study, user count)
   - Email 4 (Day 5): Address top objection
   - Email 5 (Day 7): Primary CTA (demo, trial, purchase)
   - Branch after email 3: if clicked → accelerate to CTA. If no opens → switch to re-engagement.

2. **Build abandoned cart sequence** — 3 emails recovering purchases:
   - Email 1 (1 hour): Reminder with product image. No discount. Subject: "[Name], you left something behind"
   - Email 2 (24 hours): Add social proof or scarcity. Subject: "Still thinking about [product]?"
   - Email 3 (72 hours): Offer incentive if margin allows (10% off, free shipping). Subject: "Last chance + [incentive]"
   - Exit on purchase at any point (suppression trigger).

3. **Build B2B lead nurture** — 5-7 emails qualifying leads for sales:
   - Email 1 (Day 0): Welcome + link to high-value content piece
   - Email 2 (Day 2): Educational content matching their pain point
   - Email 3 (Day 5): Case study relevant to their industry/role
   - Email 4 (Day 8): Comparison or ROI calculator
   - Email 5 (Day 12): Invite to demo/webinar
   - Email 6 (Day 16): Direct ask — "Ready to talk?"
   - Email 7 (Day 21): Soft close or park in newsletter
   - Branch: If demo booked at any point → exit to sales sequence.

4. **Deliverability audit** — Check sending infrastructure:
   - Verify SPF, DKIM, DMARC records
   - Check sender reputation (Google Postmaster Tools, Microsoft SNDS)
   - Review bounce rate trend (target <2%)
   - Review spam complaint rate (target <0.1%)
   - Check list hygiene (% inactive, last engagement date)

## Tips

- The welcome email is your highest-performing email. It gets 50-60% open rates. Don't waste it on "thanks for signing up" — deliver real value immediately.
- Abandoned cart email 1 should NOT include a discount. Most cart abandoners just got distracted. Remind first, discount last.
- Subject line and preheader are 80% of the open decision. Test these relentlessly.
- Single CTA per email. Multiple options = decision paralysis = no clicks.
- Segment by acquisition source. A lead from a Google Ads demo request needs different nurture than one from a Facebook lead form.
- Email-to-landing-page message match matters here too. The CTA text should match the landing page headline exactly.

## Gotchas

- **Over-mailing new subscribers** — Sending daily during week 1 causes unsubscribes. Space welcome sequences 1-3 days apart.
- **No suppression triggers** — If someone purchases, they must exit the cart abandonment sequence immediately. Missing suppression = sending "come back" emails to paying customers.
- **Ignoring deliverability** — Sending to a cold list on a new domain = spam folder. Always warm up.
- **Generic from name** — "noreply@company.com" kills trust. Use a real person's name or a branded name that matches the ad experience.
- **Testing too many things** — One variable per test. Changing subject line AND send time simultaneously makes results unreadable.
- **Letting sequences run forever** — Audit performance monthly. Sequences that worked at launch may fatigue after 6 months as the audience composition shifts.

## References

- `references/sequence-templates.md` — complete email sequence blueprints with timing, copy frameworks, and branching logic for all 6 sequence types

## Related Modules

- **landing-pages** — email-to-landing-page message match, form optimization for email capture
- **paid-search** / **paid-social** — acquisition campaigns that feed email sequences
- **analytics** — email attribution, cross-channel measurement of email's role in conversion path
