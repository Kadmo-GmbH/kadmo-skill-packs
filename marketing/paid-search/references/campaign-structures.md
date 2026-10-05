# Campaign Structure Templates

Ready-to-use campaign structures for common business types. Adapt to the specific account using `media-context.md`.

## Ad Group Architecture Frameworks

| Framework | Structure | When to Use | 2025+ Status |
|-----------|-----------|-------------|-------------|
| **SKAG** (Single Keyword Ad Groups) | 1 keyword per ad group, exact match | High-volume terms with 2%+ of total conversions and 500+ impressions/week | Declining — Smart Bidding reduces effectiveness |
| **STAG** (Single Theme Ad Groups) | Related keywords grouped by intent | Default for most campaigns with Smart Bidding | **Current recommended default** |
| **SIAG** (Single Intent Ad Groups) | Keywords grouped by searcher intent | Smart Bidding with broad/phrase match | Emerging best practice |
| **Hagakure** | Ad groups by destination URL, not keywords | Large accounts ($10K+/mo), high impression volume | Preferred for automation-first accounts |

**Selection guide by budget:**
- Small/medium (<$10K/mo): SIAGs only, add SKAGs for proven high-performers
- Large ($10K+/mo): Hagakure for URLs with 3,000+ monthly impressions, SIAGs/DSAs for rest, SKAGs for top performers

## Budget Allocation: The 70-15-15 Rule (Google Ads)

| Tier | Allocation | What Goes Here | Graduation Criteria |
|------|-----------|----------------|-------------------|
| **Foundation** | 70% (min 60%) | Proven winners, high ROAS, consistent converters | — |
| **Bridge** | 15% | Safe expansions (geo mods, service add-ons, question keywords) | Delivers Foundation-comparable ROAS |
| **Frontier** | 15% (min 10%) | New tests, competitor keywords, adjacent services | 100+ clicks AND meets conversion targets |

Rebalance on a 30-day cycle: Evaluate Frontier at 100+ clicks, promote to Bridge if converting. Promote Bridge to Foundation when ROAS matches.

## SaaS / B2B Lead Gen

```
Campaign: {brand}_leads_search_{geo}_brand
  Ad Group: brand-exact       [brand name, brand + product]
  Ad Group: brand-misspellings [common misspellings]

Campaign: {brand}_leads_search_{geo}_nonbrand-core
  Ad Group: {product-category}_exact    [crm software, crm platform]
  Ad Group: {product-category}_phrase   "crm software" "crm platform"
  Ad Group: {use-case}_exact            [manage customer relationships]
  Ad Group: {use-case}_phrase           "manage customer relationships"

Campaign: {brand}_leads_search_{geo}_nonbrand-long-tail
  Ad Group: {pain-point}_phrase         "too many spreadsheets for contacts"
  Ad Group: {comparison}_phrase         "salesforce alternative" "hubspot vs"

Campaign: {brand}_leads_search_{geo}_competitor
  Ad Group: {competitor-1}_exact        [competitor name]
  Ad Group: {competitor-2}_exact        [competitor name]
```

**Budget split:** Brand 10-15% | Non-brand core 50-60% | Long-tail 15-20% | Competitor 10-15%

**Bidding:** Brand → Target IS (90%) | Non-brand → tCPA | Competitor → tCPA (higher target, lower conv rate)

---

## Ecommerce

```
Campaign: {brand}_sales_shopping_{geo}_allproducts
  → Standard Shopping or Performance Max (feed-based)

Campaign: {brand}_sales_search_{geo}_brand
  Ad Group: brand-exact
  Ad Group: brand-product     [brand + product name]

Campaign: {brand}_sales_search_{geo}_category
  Ad Group: {category-1}_exact    [running shoes, running sneakers]
  Ad Group: {category-1}_phrase   "running shoes" "running sneakers"
  Ad Group: {category-2}_exact    [trail running shoes]

Campaign: {brand}_sales_search_{geo}_product
  Ad Group: {product-1}_exact     [specific product name + model]
  Ad Group: {product-2}_exact     [specific product name + model]

Campaign: {brand}_sales_search_{geo}_generic
  Ad Group: {generic-intent}_broad    best shoes for running (with smart bidding)
```

**Budget split:** Shopping/PMax 40-50% | Brand 10% | Category 25-30% | Product 10-15% | Generic 5-10%

**Bidding:** Shopping → tROAS | Brand → Target IS | Category → tROAS | Product → tROAS | Generic → Max conversions (capped)

---

## Local Business / Service

```
Campaign: {brand}_leads_search_{city}_brand
  Ad Group: brand-exact
  Ad Group: brand-service     [brand + plumber/dentist/etc]

Campaign: {brand}_leads_search_{city}_service-core
  Ad Group: {service}_exact        [plumber near me, emergency plumber]
  Ad Group: {service}_phrase       "plumber in {city}"
  Ad Group: {service-type}_exact   [pipe repair, drain cleaning]

Campaign: {brand}_leads_search_{city}_service-urgent
  Ad Group: {emergency}_exact      [emergency plumber, 24 hour plumber]
  Ad Group: {emergency}_phrase     "emergency plumber {city}"
```

**Budget split:** Brand 10% | Core service 60% | Urgent/emergency 30%

**Bidding:** Brand → Target IS | Core → tCPA | Urgent → tCPA (higher budget, time-sensitive)

**Extensions:** Location, call, structured snippets (services), callouts (24/7, licensed, insured)

---

## RSA Headline Templates

15 headlines per ad group, organized by type:

| Slot | Type | Template | Example |
|------|------|----------|---------|
| 1 | Keyword | {Primary keyword} — direct match | CRM Software for Teams |
| 2 | Keyword | {Keyword + qualifier} | Best CRM Software 2025 |
| 3 | Keyword | {Keyword + audience} | CRM for Small Business |
| 4 | Benefit | {Outcome verb} + {result} | Close 40% More Deals |
| 5 | Benefit | {Save/reduce} + {pain point} | Save 10 Hours a Week |
| 6 | Benefit | {Achieve} + {outcome} | Grow Revenue Faster |
| 7 | Social proof | {Number} + {users/companies} | Trusted by 5,000+ Teams |
| 8 | Social proof | {Award/rating} | Rated #1 on G2 |
| 9 | CTA | {Action} + {offer} | Start Your Free Trial |
| 10 | CTA | {Action} + {time qualifier} | Get Started in 5 Minutes |
| 11 | Offer | {Price/discount} | Plans from $29/month |
| 12 | Offer | {Free + qualifier} | Free 14-Day Trial |
| 13 | Brand | {Brand name} + {category} | Acme — Modern CRM |
| 14 | Dynamic | {KeyWord:Default Text} | {KeyWord:CRM Software} |
| 15 | Differentiator | {Unique feature/claim} | No Credit Card Required |

**Description templates (4 per ad group):**

| Slot | Type | Template |
|------|------|----------|
| 1 | Benefit + CTA | {Primary benefit}. {Supporting detail}. {CTA with offer}. |
| 2 | Feature + proof | {Key feature} that {outcome}. {Social proof point}. |
| 3 | Trust + CTA | {Trust signal}. {Differentiator}. {CTA}. |
| 4 | Urgency/offer | {Time-limited offer or reason to act now}. {What they get}. {CTA}. |

---

## Negative Keyword Starter Lists

### Universal negatives (add to all non-brand campaigns):
```
free, cheap, diy, how to, what is, definition, meaning, tutorial,
jobs, careers, hiring, salary, resume, certification, course, class,
review, reviews, reddit, quora, complaint, scam, lawsuit
```

### B2B SaaS negatives:
```
open source, self hosted, download, crack, torrent, template,
student, personal, hobby, excel, spreadsheet, google sheets
```

### Ecommerce negatives:
```
used, refurbished, repair, fix, manual, instructions, recall,
wholesale, bulk, dropship, amazon, ebay, walmart
```

### Local service negatives:
```
diy, how to, training, school, license, certification, supplies,
equipment, tools, wholesale, jobs, hiring, salary
```
