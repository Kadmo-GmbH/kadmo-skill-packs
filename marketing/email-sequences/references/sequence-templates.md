# Email Sequence Templates

Complete blueprints for performance marketing email sequences. Each template includes timing, subject line formulas, body structure, branching logic, and benchmarks.

## Welcome Sequence (Lead Gen / SaaS)

**Trigger:** Form submission, signup, gated content download
**Goal:** Activate → build trust → first conversion
**Length:** 5 emails over 7 days

| # | Timing | Subject Formula | Body Focus | CTA |
|---|--------|----------------|------------|-----|
| 1 | Immediate | "Here's your [asset] + what's next" | Deliver promised content, set expectations for sequence | Access asset |
| 2 | Day 1 | "[Name], one thing about [topic]..." | Single helpful insight, establish expertise | Read/watch resource |
| 3 | Day 3 | "How [customer] achieved [outcome]" | Social proof — case study or testimonial | See full story |
| 4 | Day 5 | "The #1 question about [topic]" | Address primary objection with proof | Learn more |
| 5 | Day 7 | "[Name], ready to [desired outcome]?" | Direct CTA to primary conversion | Start trial / Book demo |

**Branching:**
- After email 3: If clicked → skip to email 5 (ready for CTA)
- After email 5: If no opens on emails 3-5 → move to re-engagement sequence
- On conversion at any point → exit and move to post-purchase/onboarding

**Benchmarks:** Email 1 open rate: 50-60%. Sequence conversion rate: 5-15% to primary CTA.

---

## Abandoned Cart Sequence (Ecommerce)

**Trigger:** Product added to cart, no purchase within 1 hour
**Goal:** Recover purchase
**Length:** 3 emails over 72 hours

| # | Timing | Subject Formula | Body Focus | CTA |
|---|--------|----------------|------------|-----|
| 1 | 1 hour | "[Name], you left [product] behind" | Product image, price, simple reminder. NO discount. | Complete purchase |
| 2 | 24 hours | "Still thinking about [product]?" | Add social proof (review count, star rating) or scarcity ("X left in stock") | Return to cart |
| 3 | 72 hours | "Last chance: [incentive] on [product]" | Offer incentive if margin allows (10% off, free shipping). Create urgency. | Claim offer |

**Timing by AOV (Attribuly 2026 benchmarks):**

| AOV Tier | Email 1 | Email 2 | Email 3 |
|----------|---------|---------|---------|
| Low (<$50) | 1-2 hours | ~24 hours | ~48 hours |
| Mid ($50-$200) | 2-4 hours | 24-36 hours | ~72 hours |
| High (>$200) | 4-8 hours | 24-48 hours | 72-96 hours |

Optimal first-email window: **45-90 minutes** post-abandonment. Under 30 min risks messaging while still on site.

**Branching:**
- Purchase at any point → exit immediately (suppression trigger critical)
- High-AOV cart → skip discounts, compress timing, different messaging
- Email 1 opened but not clicked → send email 2 with stronger subject
- No opens on email 1-2 → email 3 uses different subject angle (curiosity instead of product name)
- Anonymous/unidentified user → wait 2h for identity resolution; if unmatched, shift to retargeting ads

**Recovery rates by AOV (Shopify 2025):**

| AOV | Average Recovery | Top Performers |
|-----|-----------------|----------------|
| Under $50 | 7-10% | 12-15% |
| $50-$100 | 10-13% | 15-19% |
| $100-$200 | 13-16% | 19-24% |
| $200-$500 | 16-20% | 24-30% |
| Over $500 | 18-24% | 28-35% |

**Benchmarks:** Email 1 open rate: 40-50%. Overall recovery rate: 10.7% (Shopify avg). Revenue per email: highest of any automated sequence. Top 10% of brands achieve 65% open rate and $28.89 RPR.

**Critical rules:**
- Never discount in email 1 (most abandoners just got distracted)
- Include product image and price in every email
- Show free shipping threshold if applicable
- Mobile-optimized (most cart additions happen on mobile)

---

## B2B Lead Nurture Sequence

**Trigger:** Lead form submission, content download, webinar registration
**Goal:** Educate → qualify → hand off to sales
**Length:** 7 emails over 21 days

| # | Timing | Subject Formula | Body Focus | CTA |
|---|--------|----------------|------------|-----|
| 1 | Day 0 | "Your [asset] + a quick thought on [topic]" | Deliver asset, introduce one key insight | Access content |
| 2 | Day 2 | "[Number] ways to [solve their problem]" | Educational, practical, no selling | Read article |
| 3 | Day 5 | "How [company in their industry] [achieved outcome]" | Case study — specific numbers, relevant industry | See results |
| 4 | Day 8 | "[Product category]: [yours] vs. the alternatives" | Comparison or ROI calculator. Fair and specific. | Try calculator |
| 5 | Day 12 | "[Name], see [product] in 15 minutes?" | Demo or webinar invitation. Low commitment. | Book demo |
| 6 | Day 16 | "Quick question, [Name]" | Direct ask — are they evaluating solutions? Reply encouraged. | Reply |
| 7 | Day 21 | "[Name], here when you're ready" | Soft close. Offer to stay in touch. No pressure. | Subscribe to newsletter |

**Branching:**
- Demo booked at any point → exit to sales handoff sequence
- Replies to email 6 → route to sales team immediately
- No opens on emails 1-4 → try subject line variant on email 5
- Opens but no clicks → they're reading but not ready. Continue sequence.
- No engagement by email 7 → park in monthly newsletter, re-evaluate in 90 days

**Benchmarks:** Open rates: 20-30%. CTR: 2-5%. Demo book rate from sequence: 3-8%.

---

## Post-Purchase Sequence (Ecommerce)

**Trigger:** Purchase completed
**Goal:** Onboard → reduce buyer's remorse → cross-sell → review
**Length:** 5 emails over 30 days

| # | Timing | Subject Formula | Body Focus | CTA |
|---|--------|----------------|------------|-----|
| 1 | Day 0 | "Your order is confirmed + what's next" | Order details, shipping timeline, what to expect | Track order |
| 2 | Day 3 | "[Product] tip: [quick win]" | Helpful usage tip or setup guide | Read guide |
| 3 | Day 7 | "How's [product] working for you?" | Check-in + link to support. Build relationship. | Get help / Leave review |
| 4 | Day 14 | "Customers who bought [product] also love..." | Cross-sell recommendation. Personalized to category. | Shop related |
| 5 | Day 30 | "It's been a month — [replenishment/upgrade CTA]" | Replenishment reminder or loyalty program intro | Reorder / Join loyalty |

**Branching:**
- Review submitted → send thank-you, skip future review asks
- Cross-sell purchased → update product recommendations
- Support request → pause marketing emails, route to support sequence

---

## Re-engagement Sequence

**Trigger:** No opens or clicks in 60-90 days
**Goal:** Win back or clean list
**Length:** 3 emails over 10 days

| # | Timing | Subject Formula | Body Focus | CTA |
|---|--------|----------------|------------|-----|
| 1 | Day 0 | "We miss you, [Name]" (or "Is this still your thing?") | Remind value, ask if they want to stay | Re-confirm interest |
| 2 | Day 5 | "Here's what you've missed" | Best content/product since they disengaged | Browse updates |
| 3 | Day 10 | "Last email unless you say stay" | Clear: this is the final email. Click to stay. | Stay subscribed |

**Branching:**
- Click on any email → move back to active segment, reset engagement clock
- No engagement after email 3 → remove from active list (improves deliverability)

**Benchmarks:** Re-engagement rate: 5-15% of dormant contacts. The real value is list cleaning — removing non-engagers improves deliverability for everyone else.

---

## Branching Logic Patterns

Standard conditions used across all sequences:

| Condition | Branch |
|-----------|--------|
| **Opened** | Continue sequence normally |
| **Clicked** | Accelerate to CTA (may skip educational emails) |
| **Converted** | Exit sequence immediately (suppression) |
| **Replied** | Route to human (sales team or support) |
| **No opens (2+ emails)** | Try subject line variant, then move to re-engagement |
| **Unsubscribed** | Remove from all marketing sequences |
| **Bounced (hard)** | Remove from list immediately |
| **Bounced (soft, 3x)** | Remove from list |
| **Spam complaint** | Remove from all sends, investigate root cause |

## Email-to-Landing-Page Match Rules

For any email with a CTA linking to a landing page:

| Email Element | Must Match On Page |
|--------------|-------------------|
| CTA text | Landing page headline or button text |
| Offer | Exact same offer (not a different one) |
| Visual style | Same brand colors, imagery approach |
| Tone | Same formality level |

Mismatch between email CTA and landing page = 50-80% conversion loss (MarketingExperiments).
Dedicated landing pages convert 65% better than sending email traffic to homepage (Unbounce).
