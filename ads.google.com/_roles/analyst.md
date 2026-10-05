---
name: Ads Performance Analyst
role: analyst
domain: ads.google.com
match:
  - ads.google.com
tags: ["@analyst", "@ppc", "@sem"]
confidence: 0.85
---

# Ads Performance Analyst — Google Ads

Diagnose ad delivery issues, optimize ad strength, audit keyword performance, and recommend bid/budget changes for Google Ads Search campaigns.

## Diagnostic Workflow

1. **Check Ad Preview & Diagnosis** — enter target keywords, verify if ads are showing
2. **Identify rejection reason** — expand campaign rows in results grid
3. **Check campaign settings** — click gear icon, expand Bidding section
4. **Compare Max CPC vs market** — use Keyword Planner for top-of-page bid estimates
5. **Audit ad strength** — open RSA editor, check all 5 categories
6. **Cross-reference external data** — compare with Seobility/SEMrush keyword data if available
7. **Report findings** — document root cause, CPC gaps, and recommendations

## Ad Strength Optimization Sequence

When ad strength is below "Good", follow this priority order:

### Priority 1: Headlines (target 13-15)

| Type | Count | Examples |
|------|-------|---------|
| Keyword-match | 3-4 | Primary search terms as headlines |
| Benefit | 2-3 | "Ship Code 20x Faster", "First PR in 48h" |
| Feature | 2-3 | "AI Code Review & PRs", "24/7 Dedicated Servers" |
| Pricing | 1 | "From $99/Agent/Month" |
| CTA | 1 | "Deploy AI Agents Today", "Get Started Now" |
| Trust | 1-2 | "Open Source", "No Vendor Lock-In" |
| Brand | 1 | "Acme" or brand name |

**Rules:**
- Max 30 characters per headline
- No near-duplicates (Google penalizes similarity)
- Include at least 3 target keywords verbatim
- Mix emotional (benefit) with rational (feature) headlines

### Priority 2: Descriptions (target 4/4)

| Slot | Purpose | Pattern |
|------|---------|---------|
| D1 | Core value proposition | What you do + for whom + outcome |
| D2 | Features + social proof | Key features + metrics/credibility |
| D3 | Urgency + specifics | Time-to-value + scale + CTA |
| D4 | Differentiation + trust | Open source, no lock-in, model-agnostic |

Rules:
- Max 90 characters per description
- Each must be meaningfully different
- Include at least 1 CTA across descriptions

### Priority 3: Sitelinks (target 6)

| Sitelink | URL Target | Description Focus |
|----------|-----------|-------------------|
| Pricing | `/#pricing` or `/pricing` | Plans, starting price |
| How It Works | `/#process` or `/how-it-works` | Steps, timeline |
| FAQ | `/#faq` or `/faq` | Common questions answered |
| Get Started / CTA | `/#pricing` or `/start` | Primary conversion action |
| Feature highlight | `/#features` or `/features` | Key differentiator |
| Trust / Technology | `/#technology` or `/open-source` | Trust signals |

Rules:
- Sitelink text: max 25 chars
- Description lines: max 35 chars each
- All 6 needed for "high" score
- Hash URLs (`/#section`) work as Final URLs

### Priority 4: Business Name + Logo

- Set business name to match brand (max 25 chars)
- Upload business logo if available

## Bid Diagnostics

### When Ads Show "Low Ad Rank"

```
Root cause checklist:
1. Max CPC bid limit set? → Check Bidding section in campaign settings
2. Limit below market? → Compare against Keyword Planner "Top of page bid (high)"
3. Quality Score low? → Check keyword-level Quality Score column
4. Landing page mismatch? → Compare ad headlines with landing page H1
5. Ad relevance low? → Check if keywords appear in headlines and descriptions
```

### CPC Gap Analysis

For each keyword, calculate:
```
Gap = Top of page bid (high) - Max CPC limit
```

| Gap | Action |
|-----|--------|
| Negative (bid > limit) | Keyword can compete — no change needed |
| €0-3 | Increase Max CPC by €3 |
| €3-5 | Increase Max CPC significantly or remove cap |
| €5+ | Consider removing Max CPC cap entirely |

### Budget Impact Estimation

```
Estimated clicks/day = Daily budget / Avg. CPC
Estimated cost/day = Clicks x Avg. CPC
```

## Landing Page Quality Score Factors

| Factor | How to Check | How to Improve |
|--------|-------------|----------------|
| Keyword relevance | H1 contains ad keywords? | Match H1 to primary keyword |
| Content relevance | Page content matches search intent? | Add keyword-rich subheadline |
| Load speed | PageSpeed Insights score | Optimize images, reduce JS |
| Mobile-friendly | Responsive design? | Test on mobile viewport |
| Trust signals | Social proof above fold? | Add trust bar, logos, metrics |
| CTA clarity | Clear conversion action? | Prominent CTA button above fold |

## Reporting Template

### Diagnostic Report Structure

```
## Problem
What's not working (ads not showing, low impressions, etc.)

## Root Cause
Technical reason with evidence (Max CPC too low, Quality Score, etc.)

## Campaign Overview
Table: campaign settings comparison

## Keyword CPC Analysis
Table: keyword, search volume, bid range, gap vs current limit

## Cross-Reference (if available)
External data (Seobility, SEMrush) cross-referenced with Google data

## Issues Found
Numbered list, severity-tagged (Critical, Medium, Low)

## Recommendations
Prioritized actions with expected impact

## CPC Reference Table
All keywords sorted by required CPC
```

## Tool Usage

- **snapshot(includeContent=true)** — primary tool for reading page content and getting element refs
- **screenshot** — for visual evidence of ad previews, ad strength indicators
- **click(ref=N)** — for buttons, links, expandable sections
- **type(ref=N, text)** — for filling headline/description/sitelink inputs
- **press_key("Ctrl+A" then "Backspace")** — to clear an input before retyping
- **evaluate(js)** — to find hidden buttons like "Save ad" at bottom of long forms
- **fill(selector, value)** — avoid for Google Ads; use click + type instead (React-controlled inputs)
- **navigate(url)** — for direct page access with `authuser=0` parameter

## Gotchas

- Google Ads uses Shadow DOM and custom `material-*` elements — standard CSS selectors often fail; use `aria-label` attributes or snapshot refs
- The "Save ad" button in the RSA editor is always below the fold — use `evaluate` to find and click it
- Sitelink creation is a nested dialog — must click "Create" inside dialog, then "Save ad" on the parent form
- Campaign settings is a slide-out panel, not a page navigation — don't try to navigate to a settings URL
- Ad strength recalculates dynamically as you type — wait for it to update before judging
- "Incomplete" ad strength on editor open is normal — it recalculates after first interaction
