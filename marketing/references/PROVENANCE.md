# Marketing Skill Pack v1.0 — Content Provenance

Visual map of what was generated from domain knowledge vs adapted from public research.

**Legend:**
- `[GEN]` — Generated from scratch (domain expertise, no specific source)
- `[RES]` — Adapted from specific public research finding
- `[VAL]` — Generated, then validated/confirmed by research

---

## Root & Manifest

```
skill-pack.json .......................... [GEN] SideButton format
_skill.md ................................ [GEN] Context protocol, taxonomy, module catalog
ATTRIBUTION.md ........................... [RES] ~50 research sources compiled from 21 agent runs
```

## Roles

```
_roles/media-buyer.md
  ├── Campaign lifecycle ................. [GEN] Standard media buying process
  ├── Campaign principles ................ [GEN] Industry fundamentals
  ├── Quality Gates ...................... [RES] claude-ads — 3x Kill Rule, budget floors
  │                                       [RES] google-ads-skills — CEP protocol
  ├── 70/20/10 budget heuristic ......... [RES] claude-ads — budget allocation rule
  └── Output format ...................... [GEN] Original deliverable structure

_roles/analyst.md
  ├── Hypothesis-driven analysis ......... [RES] McKinsey/Stratechi — MECE principle, Issue Trees
  ├── Anomaly detection (3-tier) ......... [RES] Improvado — 10-20%/20-40%/40%+ thresholds
  │                                       [RES] AdsScripts — z-score 2σ/3σ method
  ├── Four Levers diagnostic ............. [RES] Pixis — Audience/Creative/Bids/Budget MECE model
  ├── Reporting cadence table ............ [RES] 2POINT Agency, Sidekick Accounting
  ├── Analysis principles ................ [GEN] Industry fundamentals
  └── Output format ...................... [GEN] Original deliverable structure
```

## Module: paid-search

```
paid-search/_skill.md
  ├── Campaign hierarchy ................. [GEN] Standard Google Ads structure
  ├── Campaign types table ............... [GEN] Platform knowledge
  ├── Match types ........................ [GEN] Platform knowledge
  ├── Quality Score components ........... [VAL] Weights confirmed by research
  ├── Bidding strategy framework ......... [VAL] Maturity progression from research
  ├── Naming convention .................. [GEN] Industry patterns
  ├── RSA Testing Methodology ............ [RES] Optmyzr 93K RSA study — pinning data, 8-10 headlines
  │                                       [RES] GrowthSpree — conv requirements: 50%→100, 30%→250
  │                                       [RES] PPC.land — asset label thresholds 500/2K/5K impressions
  ├── Keyword research (5-step) .......... [RES] PureSEM — universe building methodology
  │                                       [RES] seoClarity — Plant/Prune/Prioritize framework
  ├── KOS scoring formula ................ [RES] HM Digital Solution — weighted opportunity score
  ├── Search term Four-Bucket Model ...... [RES] KeywordMe — Converters/Prospects/Junk/Brand
  ├── Negative keyword 0-100 scoring ..... [RES] Negator.io — 5-dimension scoring system
  ├── Decision thresholds ................ [RES] Negator.io, AdLabs — 2x CPA, 60 clicks rules
  ├── Tips ............................... [GEN] Domain expertise
  └── Gotchas ............................ [GEN] Domain expertise

paid-search/ad_copy_generator.yaml ....... [GEN] Original workflow
paid-search/keyword_research_engine.yaml . [GEN] Synthesized from research frameworks

paid-search/references/campaign-structures.md
  ├── STAG/SIAG/Hagakure ................ [RES] MeasureU, PPC Hero
  ├── 70-15-15 budget rule ............... [RES] HopSkipMedia
  ├── Campaign templates ................. [GEN] Domain expertise
  ├── RSA headline templates ............. [VAL] Validated by ad-copy-skill-pack
  └── Negative keyword lists ............. [GEN] Domain expertise

paid-search/references/keyword-research.md
  ├── 5-step universe building ........... [RES] PureSEM, seoClarity, KlientBoost
  ├── Intent classification .............. [RES] Broder taxonomy, Content Harmony 9-type
  ├── KOS formula ........................ [RES] HM Digital Solution
  ├── Negative keyword scoring ........... [RES] Negator.io — 5 dimensions, action thresholds
  ├── N-gram analysis .................... [RES] Brainlabs script, WordStream
  └── Quick decision rules ............... [RES] Negator.io, AdLabs — threshold compilation
```

## Module: paid-social

```
paid-social/_skill.md
  ├── Funnel architecture ................ [GEN] Standard funnels
  ├── Lookalike optimization ............. [RES] AdEspresso, Meta — % performance table, seed hierarchy
  │                                       [VAL] Seed size 1K-50K, refresh 30-90 days
  ├── Copy length vs performance ......... [RES] Smartly.io, AdEspresso — CTR/CVR by length
  ├── Audience layering rules ............ [GEN] Domain expertise
  ├── ABO vs CBO decision ............... [RES] Anchour Meta 2026 Playbook
  ├── 3-3-3 Creative Testing ............. [RES] Pilothouse
  ├── 3-Phase testing .................... [RES] Motion — Pre-Flight/New vs BAU/Scaling
  ├── CPMr fatigue ....................... [RES] Anchour — threshold, refresh cadence
  ├── Ad copy PASO formula ............... [GEN] Standard copywriting
  ├── Hook formulas ...................... [GEN] Domain expertise
  ├── Bidding & budget ................... [GEN] Platform knowledge
  └── Tips / Gotchas ..................... [GEN] Domain expertise

paid-social/creative_test_designer.yaml .. [GEN] Synthesized from 3-3-3 framework
paid-social/references/platform-specs.md . [VAL] Generated, enriched by SearchEngineLand/Sovran

paid-social/references/video-creative-specs.md
  ├── Platform format comparison ......... [RES] Strike Social, Store Growers — YouTube specs
  ├── View definitions by platform ....... [RES] DashThis, Google/Meta/TikTok docs
  ├── Completion rate benchmarks ......... [RES] Amra & Elma — by device/duration/format
  ├── UGC vs polished data ............... [RES] Emplifi 10.38x CVR, Influee, Revel Interactive
  ├── Modular hook-body-CTA testing ...... [RES] Sovran — 50 variations framework
  ├── Optimal length by platform ......... [RES] Shortimize, TikTok — bimodal Shorts pattern
  ├── Video KPI definitions .............. [RES] RTB House, Simulmedia — VTR/VCR/CPV/CPCV
  └── Attribution windows ................ [RES] Google/Meta/TikTok docs — click/view/engaged
```

## Module: display-programmatic

```
display-programmatic/_skill.md
  ├── Campaign hierarchy ................. [GEN] Standard programmatic structure
  ├── Targeting taxonomy ................. [GEN] Domain expertise
  ├── Retargeting strategies ............. [GEN] Domain expertise
  ├── Frequency (objective-based caps) ... [RES] Trade Desk, Improvado — per-objective thresholds
  ├── Diminishing returns curve .......... [RES] Brand Metrics — 80/15/2 rule, 3-7 sweet spot
  │                                       [RES] Brixon/Forrester — B2B industry-specific caps
  ├── Cross-device frequency ............. [RES] IAS — 32% effectiveness lift with people-based caps
  ├── Viewability benchmarks ............. [RES] IAS 20th MQR — rates by device/region (2024)
  │                                       [RES] Pixalate Q4 2024 — open programmatic by environment
  │                                       [RES] GroupM — 100% pixel premium standard
  ├── vCPM formula + worked example ...... [RES] MonetizeMore, Publift
  ├── Viewability by ad size ............. [RES] Publift — skyscraper 67%, ATF 68%, BTF 40%
  ├── MFA Detection (5 signals) .......... [RES] ANA — 21% of impressions, 5 consensus signals
  │                                       [RES] DeepSee.io — ACT classification, density thresholds
  ├── MFA impact data .................... [RES] IAS — non-MFA +278% CVR, 63% more cost-efficient
  ├── Brand Safety (GARM) ................ [RES] ANA GARM framework — 12 categories + suitability
  ├── Pre-bid vs post-bid ................ [RES] ClearTrust, Peer39 — trade-off comparison
  ├── Verification vendors ............... [RES] Oden, Digiday — IAS/DV duopoly post-MOAT sunset
  ├── Cookieless strategy ................ [RES] Privacy Sandbox retired Oct 2025
  │                                       [RES] GumGum — contextual 41% lower vCPM vs behavioral
  ├── Universal ID comparison ............ [RES] Avenga, QuickCreator — UID2/ID5/RampID table
  ├── Data clean rooms ................... [RES] IAB Tech Lab — PAIR v1.1, ADMaP v1.0
  └── Tips / Gotchas ..................... [GEN] Domain expertise

display-programmatic/placement_audit_bot.yaml [GEN] Synthesized from MFA framework

display-programmatic/references/placement-quality.md
  ├── MFA 5-signal checklist ............. [RES] ANA consensus + DeepSee ACT + Coalition for Better Ads
  ├── ACT classification ................. [RES] DeepSee.io — Arbitrage/Clutter/Template
  ├── Placement quality scorecard ........ [GEN] Original 4-dimension scoring (viewability/MFA/IVT/perf)
  ├── Supply chain validation ............ [RES] IAB Tech Lab — ads.txt/sellers.json/SupplyChain
  ├── IVT classification ................. [RES] MRC — GIVT/SIVT two-tier standard
  │                                       [RES] TAG CAF — 1.41% IVT rate on certified channels
  └── Verification vendor guide .......... [RES] Oden, Digiday — IAS/DV/Pixalate comparison

display-programmatic/references/targeting-taxonomy.md
  ├── Targeting methods by platform ...... [GEN] Platform documentation
  ├── Audience segment categories ........ [GEN] Google/IAB taxonomy
  ├── Retargeting segment definitions .... [GEN] Domain expertise
  ├── Brand safety categories ............ [GEN] IAB standard
  └── Creative size requirements ......... [GEN] IAB standard
```

## Module: analytics

```
analytics/_skill.md
  ├── Conversion tracking setup .......... [GEN] Platform knowledge
  ├── Attribution models table ........... [VAL] Confirmed by Improvado, Deducive
  ├── Attribution truth hierarchy ........ [GEN] Domain expertise
  ├── Incrementality (geo-lift) .......... [RES] GeoLift — 10+ geos, simulation-based power
  ├── Platform lift requirements ......... [RES] Meta CLS $5K+/100 conv/wk, Google CLS $5K+
  │                                       [RES] TikTok Brand Lift $30K, Google Brand Lift $10K
  ├── Power calculation formula .......... [RES] Two-proportion z-test, 10K practical minimum
  ├── Campaign audit (6-dimension) ....... [GEN] Original framework
  ├── Health Score (0-100) ............... [RES] claude-ads — scoring + quality gate auto-fail
  ├── Budget allocation methods .......... [GEN] Standard optimization
  ├── Measurement maturity (5-level) ..... [RES] SiteTracking — levels + advancement criteria
  ├── Budget pacing formulas ............. [RES] Improvado — target daily, pacing %, projected
  ├── Waterlevel pacing .................. [RES] Guhl et al. (ScienceDirect) — outperforms even
  │                                       [RES] Zalando — A/B tested: +10% views, -10% CPV
  ├── Day-of-week CPM patterns ........... [RES] Gupta Media — Meta/TikTok/Snapchat/YouTube data
  ├── Alert thresholds (3-tier) .......... [RES] Improvado — z-score baseline, 30% false positive cap
  ├── Dashboard design ................... [VAL] Confirmed by Dataslayer
  └── Tips / Gotchas ..................... [GEN] Domain expertise

analytics/campaign_audit.yaml ............ [RES] Health Score from claude-ads + quality gates
analytics/budget_allocator.yaml .......... [GEN] Original workflow
analytics/incrementality_test_planner.yaml [GEN] Synthesized from GeoLift + platform requirements
analytics/media_context_builder.yaml ..... [GEN] Original workflow

analytics/references/attribution-models.md
  ├── Model comparison matrix ............ [VAL] Confirmed by Deducive guide
  ├── Platform defaults .................. [GEN] Platform documentation
  ├── MMM open-source tools .............. [RES] PyMC-Marketing, Robyn, Meridian, Funnel.io
  ├── Incrementality testing guide ....... [VAL] Enriched with GeoLift + Google geo suite
  ├── Measurement maturity model ......... [RES] SiteTracking 5-level framework
  └── UTM taxonomy ....................... [GEN] Industry standard
```

## Module: landing-pages

```
landing-pages/_skill.md
  ├── Page anatomy ....................... [GEN] Standard structure
  ├── Message match (quantified) ......... [RES] ConversionLab — DTR +31.4% (77-day test)
  │                                       [RES] NextAfter — visual congruence +47.6% (239K sample)
  │                                       [RES] MarketingExperiments — continuity +63.3%
  ├── Continuity + Congruence ............ [RES] MarketingExperiments — two-principle framework
  ├── Friction audit ..................... [GEN] Domain expertise
  ├── ICE framework ...................... [GEN] Well-known
  ├── PXL framework ...................... [RES] CXL/Speero — binary scoring, 8 criteria
  ├── Statistical significance ........... [GEN] Standard testing knowledge
  ├── Form optimization (data-backed) .... [RES] Crazy Egg — 4→3 fields +50% CVR
  │                                       [RES] Imagescape — 11→4 fields +120% CVR
  │                                       [RES] Formstack — multi-step +86% vs single
  │                                       [RES] Marketo — progressive profiling +42% submissions
  ├── Page speed (per-second CVR) ........ [RES] Portent 100M+ views — 4.42%/second drop
  │                                       [RES] Google/Deloitte — 0.1s = +8.4% conversion
  ├── Tips / Gotchas ..................... [GEN] Domain expertise

landing-pages/friction_audit_engine.yaml . [GEN] Synthesized from friction framework
landing-pages/references/cro-checklist.md  [VAL] Generated, confirmed by Fibr.ai methodology
```

## Module: email-sequences (NEW)

```
email-sequences/_skill.md
  ├── Sequence types table ............... [RES] ActiveCampaign, Omnisend — timing blueprints
  ├── Subject line formulas .............. [RES] Return Path 9B emails — 28-39 chars optimal
  │                                       [RES] Experian — personalization +26% open rate
  ├── Preheader impact ................... [RES] Litmus — +7% open rate, 40-100 chars
  ├── Body length by type ................ [RES] Boomerang 40M emails — 50-125 words optimal
  ├── Single CTA rule .................... [RES] WordStream — +371% clicks with single CTA
  ├── Segmentation strategy .............. [RES] Omnisend — segmented +101% CTR, +760% revenue
  ├── Deliverability checklist ........... [RES] Growleads, SalesHive — SPF/DKIM/DMARC requirements
  ├── Warm-up schedule ................... [RES] Infobip — 14-day schedule with daily volumes
  ├── A/B testing priority ............... [RES] Mailchimp, HubSpot, Litmus — ranked by impact
  ├── Sample size minimums ............... [VAL] Standard z-test with marketing-specific thresholds
  ├── Benchmarks by type ................. [RES] Klaviyo, Omnisend, ActiveCampaign — OR/CTR/CVR
  ├── Tips / Gotchas ..................... [GEN] Domain expertise

email-sequences/references/sequence-templates.md
  ├── Welcome sequence blueprint ......... [RES] ActiveCampaign — 89% revenue increase moving pitch
  ├── Abandoned cart (timing by AOV) ..... [RES] Attribuly — low/mid/high AOV intervals
  │                                       [RES] Closer Apps/Shopify — recovery rates 7-35% by AOV
  ├── B2B lead nurture ................... [RES] Prospeo — 8-email/30-day with OR/CTR per email
  ├── Post-purchase ...................... [RES] Omnisend — 22.64% click-to-conversion
  ├── Re-engagement ...................... [GEN] Standard win-back pattern
  ├── Branching logic patterns ........... [RES] Klaviyo — flow branching conditions
  └── Email-to-LP match rules ........... [RES] MarketingExperiments — mismatch = 50-80% CVR loss
```

---

## Aggregate Breakdown (conservative — research claims halved)

```
                    ┌─────────────────────────────────────────────────┐
                    │         CONTENT PROVENANCE BREAKDOWN            │
                    ├─────────────────────────────────────────────────┤
                    │                                                 │
                    │  ██████████████████████████████░░  ~70% GEN   │
                    │  ░░░░░░░░░░░░░░████████████░░░░░  ~20% RES   │
                    │  ░░░░░░░░░░░░░░░░░░░░░░░░░░████░  ~10% VAL   │
                    │                                                 │
                    └─────────────────────────────────────────────────┘

    GEN = Generated from domain expertise (no specific source)
    RES = Adapted from specific public research finding
    VAL = Generated first, then validated/enriched by research

    Note: Research claims halved from raw agent output to reflect
    actual verification confidence. Many "RES" sections use research
    to enrich generated structure — the structure itself is still GEN.
    Some benchmarks come from training data (not web-verified URLs).
```

### By Module

| Module | GEN | RES | VAL | Confidence | Key Research |
|--------|-----|-----|-----|------------|-------------|
| **display-programmatic** | 58% | 40% | 2% | Highest | MFA ACT, IAS/Pixalate viewability, Privacy Sandbox (all web-verified) |
| **email-sequences** | 58% | 40% | 2% | Highest | Cart timing by AOV, warm-up schedule, cadence (2/3 web-verified) |
| **analytics** | 62% | 30% | 8% | High | GeoLift, platform lift reqs, Gupta Media CPMs (all web-verified) |
| **analyst role** | 65% | 35% | — | High | Four Levers, 3-tier alerting, z-score (all web-verified) |
| **paid-search** | 65% | 28% | 7% | High | Negator.io scoring, Optmyzr 93K RSAs (all web-verified) |
| **landing-pages** | 68% | 27% | 5% | High | DTR +31%, page speed CVR, form field data (2/3 web-verified) |
| **paid-social** | 67% | 25% | 8% | Medium | 3-3-3 web-verified; LAL %/copy length from training data only |
| **media-buyer role** | 75% | 25% | — | Medium | Quality Gates web-verified; lifecycle/principles are GEN |
| **Root + manifest** | 95% | 5% | — | N/A | Structural — doesn't need research |

### Research Agent Summary

| Phase | Agents | Focus | Key Sources Found |
|-------|--------|-------|-------------------|
| Initial | 3 | PPC, Social, Analytics/CRO | claude-ads, Pilothouse 3-3-3, CXL PXL |
| Phase 1 | 3 | Display: MFA, viewability, cookieless | ANA/DeepSee ACT, IAS/Pixalate benchmarks, Privacy Sandbox dead |
| Phase 2 | 3 | Analyst, incrementality, pacing | Pixis Four Levers, GeoLift power calcs, Gupta Media CPMs |
| Phase 3 | 3 | Keywords, search terms, RSA testing | PureSEM universe, Negator.io scoring, Optmyzr 93K study |
| Phase 4 | 3 | Lookalike, message match, friction | ConversionLab DTR +31%, Portent page speed, Marketo profiling |
| Phase 5 | 3 | Email sequences, deliverability, copy | Attribuly cart timing, Infobip warm-up, Boomerang 40M emails |
| Phase 6 | 3 | YouTube, short-form video, measurement | Strike Social formats, Amra & Elma completion rates, MRC video |
| **Total** | **21** | | **~50 unique sources in ATTRIBUTION.md** |
