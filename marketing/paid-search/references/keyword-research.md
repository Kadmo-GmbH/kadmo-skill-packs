# Keyword Research Methodology

Structured process for building and maintaining a keyword universe for paid search campaigns.

## Keyword Universe Building (5-Step Process)

### Step 1: Plant Seeds (10-20 starter keywords)

Generate seeds from three angles:

| Angle | What to Ask | Example Seeds |
|-------|-----------|---------------|
| **Product** | What do you sell? | "crm software", "project management tool" |
| **Problem** | What pain does the customer have? | "manage customer relationships", "track sales pipeline" |
| **Solution** | What outcome do they want? | "close more deals", "automate follow-ups" |

Sources for seeds: product pages, sales call transcripts, competitor homepages, customer support FAQs, Google Search Console queries.

### Step 2: Expand (200-2,000 keywords)

| Method | What | Expected Yield |
|--------|------|---------------|
| Keyword tools (Google Keyword Planner, etc.) | Volume, competition, CPC data for seeds | 50-200 per seed |
| Competitor analysis | Keywords competitors bid on | 100-500 unique |
| Search term reports (existing accounts) | Actual queries already triggering ads | 50-500 |
| "People Also Ask" mining | Question-format long-tail queries | 20-50 |
| Autocomplete suggestions | Google autocomplete from seeds | 10-30 per seed |
| Related searches | Bottom of SERP related queries | 5-10 per search |

### Step 3: Classify Intent

| Intent Level | Signals | PPC Value | Match Type |
|-------------|---------|-----------|------------|
| **Transactional** | buy, price, cost, order, near me, [brand]+[product] | Highest | Exact, Phrase |
| **Commercial Investigation** | best, review, comparison, vs, alternative, top 10 | High | Phrase, Exact |
| **Navigational** | [brand name], [product name], login, support | Medium (brand campaigns) | Exact |
| **Informational** | how to, what is, guide, tutorial, definition | Low — usually exclude from PPC | Negative or Broad (with caution) |

## Keyword Opportunity Score (KOS)

Score every keyword to prioritize the list:

```
KOS = (Monthly Volume × Intent Multiplier × Est. CVR) / (Competition Score × Est. CPC)
```

| Factor | How to Score | Scale |
|--------|-------------|-------|
| **Monthly Volume** | From keyword tool | Raw number |
| **Intent Multiplier** | Transactional=3, Commercial=2, Informational=0.5 | 0.5-3 |
| **Est. CVR** | From historical data or industry avg (2-5% search) | 0.01-0.10 |
| **Competition Score** | Low=1, Medium=2, High=3 | 1-3 |
| **Est. CPC** | From keyword tool | Raw number |

Higher KOS = better opportunity. Focus on top quartile for initial campaign build.

### Step 4: Group into Clusters

| Clustering Rule | Guideline |
|----------------|-----------|
| One intent per group | Don't mix "buy crm" and "what is crm" |
| 10-20 keywords per ad group | Tight enough for relevant RSAs |
| Similar CPC range | Prevents bid mismatch within group |
| Shared landing page | All keywords in group should map to same page |

**Naming:** `{theme}_{intent}_{match-type}` — e.g., `crm-software_transactional_exact`

### Step 5: Maintain

| Cadence | Action |
|---------|--------|
| Weekly (new campaigns) | Review search terms, add negatives, promote converters |
| Biweekly (mature) | Search term review, keyword performance check |
| Monthly | Refresh universe with new seeds from search terms, competitor changes |
| Quarterly | Full re-evaluation of keyword list vs business goals |

## Search Term Classification: Negative Keyword Scoring

Score each search term 0-100 for exclusion priority:

| Dimension | Points | What It Measures |
|-----------|--------|------------------|
| **Semantic Relevance** | 0-30 | How far from business offerings. Larger gap = higher score |
| **Conversion Probability** | 0-25 | Historical performance + predicted likelihood. Lower = higher score |
| **Cost Impact** | 0-20 | Frequency × CPC. Higher projected waste = higher score |
| **Brand Safety** | 90-100 auto | Profanity, adult, illegal content. Auto-max regardless |
| **Keyword Conflict** | -15 to -25 | Reduces score when negative would block active keywords |

**Action by score:**

| Score | Classification | Action | Timeline |
|-------|---------------|--------|----------|
| 0-20 | Relevant | No action | — |
| 21-40 | Monitor | Weekly review | 1-2 weeks |
| 41-60 | Likely irrelevant | Batch upload as negative | 1 week |
| 61-80 | High priority | Add negative | 48 hours |
| 81-100 | Critical | Immediate exclusion | Same day |

When confidence is below 85%, require human review. The 40-70 range is where most judgment calls happen.

## Quick Decision Rules

| Condition | Action |
|-----------|--------|
| Cost > 2× target CPA, 0 conversions | Add as negative |
| Clicks > 60, 0 conversions | Strong negative candidate |
| CTR < 1% with 100+ impressions | Review ad relevance; likely negative |
| Converting at or below target CPA | Promote to keyword in matching ad group |
| High volume, good CTR, no conversion data yet | Monitor 30-60 days before deciding |
| Search term matches existing keyword exactly | Already covered; no action needed |

## N-gram Analysis

For large search term reports (500+ terms), run n-gram analysis:

1. Break all search terms into 1-gram, 2-gram, and 3-gram tokens
2. Count frequency of each token
3. Calculate total cost and conversions per token
4. Flag tokens appearing in 5%+ of terms with 0 conversions and significant cost
5. Add flagged tokens as campaign-level negatives

Example: If "free" appears in 47 search terms, cost $340 total, 0 conversions → add "free" as campaign negative.
