---
name: Keyword Diagnostics
domain: ads.google.com
tags: [keywords, diagnostics, ad-rank, cpc]
confidence: 0.85
---

# Keyword Diagnostics — Ads Not Showing

Structured diagnostic workflow for investigating and resolving "ads not showing" issues in Google Ads Search campaigns.

## Diagnostic Flowchart

```
Ads not showing
  |
  v
Check Ad Preview & Diagnosis
  |
  +-- "Low Ad Rank" --> Check Max CPC vs Keyword Planner bids
  |     |
  |     +-- Max CPC < Top of page bid (low) --> Increase CPC significantly
  |     +-- Max CPC between low-high --> Increase CPC or improve Quality Score
  |     +-- Max CPC > Top of page bid (high) --> Check Quality Score
  |
  +-- "Policy violation" --> Check ad content and landing page
  |
  +-- "Paused" --> Check campaign/ad group/keyword status
  |
  +-- "Budget exhausted" --> Check daily budget vs spend
  |
  +-- "No keywords match" --> Check keyword match types and search term
```

## Step 1: Ad Preview & Diagnosis

1. Navigate: Tools > Troubleshooting > Ad preview and diagnosis
2. Enter the exact search term users would type
3. Set correct location, language, device
4. Review results grid

**Read the rejection reason carefully:**
- "rejected early on because it's unlikely to lead to an impression based on the predicted Ad Rank" = **bid too low**
- "ad isn't showing due to a policy violation" = **policy issue**
- "campaign is paused" = **campaign/ad group/keyword disabled**

## Step 2: CPC Gap Analysis

### Get Market CPCs from Keyword Planner

1. Tools > Planning > Keyword Planner > "Get search volume and forecasts"
2. Enter all campaign keywords (comma-separated)
3. Check location filter matches campaign targeting
4. Record for each keyword:
   - Avg. monthly searches
   - Competition (Low/Medium/High)
   - Top of page bid (low range)
   - Top of page bid (high range)

### Calculate Gap

| Keyword | Max CPC | Top Page High | Gap | Action |
|---------|---------|--------------|-----|--------|
| keyword1 | €2.00 | €7.12 | **€5.12** | Must increase |
| keyword2 | €2.00 | €5.67 | **€3.67** | Must increase |
| keyword3 | €2.00 | €1.94 | -€0.06 | OK, may compete |

### Decision Matrix

| Max CPC vs Top Page Bid (High) | Recommendation |
|-------------------------------|----------------|
| Max CPC < 30% of Top Page High | Remove cap or increase 3-4x |
| Max CPC = 30-60% of Top Page High | Increase to 80% of high range |
| Max CPC = 60-90% of Top Page High | Small increase, focus on Quality Score |
| Max CPC > Top Page High | CPC is fine — check Quality Score |

## Step 3: Campaign Settings Audit

Check each setting in campaign settings panel:

| Setting | Issue | Fix |
|---------|-------|-----|
| Bid strategy: Maximize clicks + low cap | Cap prevents competitive bidding | Raise cap or remove |
| Networks: Display Network on Search campaign | Dilutes Search budget | Remove Display Network |
| Location: too narrow | Low search volume | Expand targeting |
| Language: mismatch | Excludes potential searchers | Add relevant languages |
| Status: "Bid strategy learning" | Normal for 1-2 weeks | Wait, but ensure CPC is adequate |

## Step 4: Quality Score Check

If CPC is adequate but ads still don't show:

1. Navigate to Keywords page for the campaign
2. Add "Quality Score" column if not visible
3. Check component scores:
   - **Expected CTR**: Below average → improve headlines to be more compelling
   - **Ad relevance**: Below average → add target keywords to headlines
   - **Landing page experience**: Below average → improve page content, speed, mobile

### Landing Page Quick Audit

| Check | How | Fix |
|-------|-----|-----|
| H1 matches ad keywords | Read page H1 | Update H1 to include primary keyword |
| Meta description keyword-rich | Read meta tag | Add target keywords to meta description |
| Load speed | PageSpeed Insights | Optimize images, reduce JS bundles |
| Mobile responsive | Resize viewport | Fix responsive layout issues |
| Trust signals above fold | Screenshot hero | Add trust bar, logos, metrics |
| CTA visible | Screenshot hero | Add prominent CTA button |

## Step 5: Cross-Reference External Data

If available, compare Google Ads keyword data with external sources:

| Source | What to Check |
|--------|--------------|
| Seobility/SEMrush | Global search volume, CPC, competition, intent |
| Google Trends | Keyword trending direction (rising/declining) |
| Google Search Console | Organic impressions for same keywords |

### Cross-Reference Table Template

| Keyword | Campaign | Status | Google Ads CPC (DE) | External SV | External CPC | Intent |
|---------|----------|--------|-------------------|-------------|-------------|--------|
| keyword1 | Campaign A | Eligible | €1.63-€7.12 | 2,900 | $7.20 | commercial |

**Identify gaps:**
- Keywords in external data but NOT in campaigns = missed opportunities
- Keywords in campaigns but NOT in external data = potentially low-value
- High-volume keywords that are Paused = re-enable candidates

## Reporting

Document all findings in a diagnostic report:

```markdown
# Google Ads Diagnostic Report

**Date:** YYYY-MM-DD
**Account:** {account_id}
**Market:** {country} ({city})

## Problem
{What's not working}

## Root Cause
{Technical reason with evidence}

## Campaign Overview
{Table of campaign settings}

## Keyword CPC Analysis
{Table with all keywords, bid ranges, gaps}

## Issues Found
1. {Issue} — Severity: {Critical/Medium/Low}

## Recommendations
{Prioritized actions}

## CPC Reference Table
{All keywords sorted by required CPC}
```
