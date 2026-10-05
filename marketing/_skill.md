---
name: Performance Marketing & Online Advertising
domain: marketing
tags: ["@marketing", "@performance", "@advertising", "@paid-media"]
confidence: 0.35
---

# Performance Marketing & Online Advertising

End-to-end knowledge for planning, launching, optimizing, and measuring paid digital campaigns. Covers paid search, paid social, display/programmatic, analytics/attribution, and landing page optimization. Designed for autonomous agents that manage ad spend, write ad copy, structure campaigns, analyze performance, and recommend optimizations.

This pack is **brand-agnostic**. It works with any product, vertical, or budget level. Account-specific details (budgets, KPIs, creatives, audiences) are provided by the consumer at runtime via a `media-context.md` file.

## Context Protocol

Before any campaign task, the consumer must provide a `media-context.md` containing:

- **Business model** — REQUIRED. Determines how every framework adjusts:
  - `PLG` (product-led growth) — free signup, activation-driven, viral loops (e.g., Notion, Slack, Figma)
  - `Sales-led B2B` — demo requests, sales cycle weeks-months, high ACV $5K+ (e.g., Deel, Salesforce)
  - `B2C ecommerce` — direct purchase, cart-based, transaction-focused (e.g., Shopify stores)
  - `B2C subscription` — trial/signup, retention-focused (e.g., Netflix, Spotify)
  - `Hybrid` — self-serve + sales-assisted (e.g., HubSpot, Notion Enterprise)
- **Business goal** — what the campaigns should achieve (leads, purchases, signups, app installs, awareness)
- **KPIs and targets** — primary metric (CPA, ROAS, CPL) with target values
- **Budget** — monthly/daily budget, channel allocation if pre-decided
- **Audience** — ICP description, demographics, firmographics, interests, existing customer data
- **Product/offer** — what is being promoted, pricing, USPs, competitive positioning
- **Channels** — which platforms are in scope (Google, Meta, LinkedIn, TikTok, programmatic)
- **Creative assets** — available images, videos, logos, brand guidelines
- **Tracking setup** — pixel/tag status, conversion events defined, analytics platform
- **Historical data** — past campaign performance if available (benchmarks, winners/losers)

If no media context is provided, ask for one before executing. Without it, recommendations will be generic.

## Campaign Taxonomy

| Type | Objective | Key Metric | Typical Channels |
|------|-----------|------------|------------------|
| Search — Brand | Protect brand terms, capture high-intent | CPC, impression share | Google, Bing |
| Search — Non-brand | Capture demand, drive conversions | CPA, ROAS, conv. rate | Google, Bing |
| Shopping / PMax | Product-feed campaigns, ecommerce | ROAS, COS | Google, Bing |
| Social — Prospecting | Reach new audiences, generate demand | CPM, CPA, CTR | Meta, TikTok, LinkedIn |
| Social — Retargeting | Re-engage visitors, drive conversions | CPA, ROAS, frequency | Meta, TikTok, LinkedIn |
| Display — Prospecting | Awareness, reach, upper funnel | CPM, viewability, reach | GDN, DV360, programmatic DSPs |
| Display — Retargeting | Re-engage across web, sequential messaging | CPA, view-through conv. | GDN, DV360, programmatic DSPs |
| Video | Awareness, consideration, storytelling | CPV, VTR, brand lift | YouTube, Meta, TikTok |
| Lead gen | Capture contact info directly in-platform | CPL, lead quality score | Meta Lead Ads, LinkedIn Lead Gen |

## Module Catalog

| Module | Purpose | Key Frameworks |
|--------|---------|----------------|
| **paid-search** | Google/Bing search campaigns: structure, keywords, bidding, ad copy | SKAG vs themed groups, match type strategy, Quality Score, RSA best practices |
| **paid-social** | Meta/LinkedIn/TikTok campaigns: audiences, creatives, bidding | Funnel architecture, audience layering, creative testing frameworks |
| **display-programmatic** | Display ads, programmatic buying, retargeting | DSP setup, targeting taxonomy, frequency management, viewability |
| **analytics** | Measurement, attribution, reporting, optimization | Attribution models, conversion tracking, incrementality, dashboarding |
| **landing-pages** | CRO, A/B testing, message-match, page performance | Message match, friction audit, test prioritization, statistical significance |
| **email-sequences** | Post-conversion email automation: welcome, cart, nurture, post-purchase | Sequence architecture, copy frameworks, deliverability, A/B testing |

## Loading Order

1. This file (`_skill.md`) — pack overview, context protocol, taxonomy
2. Consumer's `media-context.md` — account-specific goals, budgets, audiences
3. Role file (`_roles/media-buyer.md` or `_roles/analyst.md`) — task lifecycle
4. Module `_skill.md` — channel-specific methodology
5. Module `references/` — detailed frameworks, specs, checklists (loaded on demand)

## Cross-Module Dependencies

- `analytics` is referenced by all other modules — every campaign needs measurement setup before launch
- `landing-pages` is referenced by `paid-search` and `paid-social` — ad-to-page message match is critical for Quality Score and conversion rate
- `paid-search` and `paid-social` share audience insights — search query data informs social targeting and vice versa
- `display-programmatic` retargeting depends on pixel/audience data from `paid-search` and `paid-social` campaigns
- `email-sequences` depends on `landing-pages` (form capture) and `paid-search`/`paid-social` (acquisition source segmentation)

## Workflow Catalog

| Workflow | Module | Purpose |
|----------|--------|---------|
| `campaign_audit` | analytics | Audit campaign structure, spend efficiency, and identify optimization opportunities |
| `ad_copy_generator` | paid-search | Generate RSA headlines and descriptions from product/offer brief |
| `budget_allocator` | analytics | Recommend budget allocation across channels based on historical performance |
| `creative_test_designer` | paid-social | Design 3-3-3 creative test matrix with hypothesis, kill rules, and measurement plan |
| `friction_audit_engine` | landing-pages | Score page across 7 friction types with severity-weighted rubric and prioritized fixes |
| `incrementality_test_planner` | analytics | Design incrementality test with power calculation, market selection, and analysis plan |
| `keyword_research_engine` | paid-search | Build keyword universe with intent classification, KOS scoring, and ad group clustering |
| `placement_audit_bot` | display-programmatic | Analyze placements for MFA risk, viewability, and quality scoring with exclusion list |
| `media_context_builder` | analytics | Build complete media-context.md from structured business inputs |
