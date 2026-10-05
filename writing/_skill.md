---
name: Writing Standards
domain: writing
tags: ["@writing", "@content", "@marketing"]
confidence: 0.24
---

# Writing Standards

Production-grade content creation, editing, and quality assurance for marketing websites. Provides copywriting frameworks, structured editing methodology, content strategy planning, social content creation, and AI writing pattern detection.

This pack is **brand-agnostic**. It works with any website or product. Brand-specific voice, tone, and constraints are provided by the consumer at runtime via a `brand-context.md` file.

## Brand Context Protocol

Before any content task, the consumer must provide a `brand-context.md` containing:

- **Site identity** — name, one-liner, target audience
- **Voice rules** — tone, personality, formality level (casual / professional / formal)
- **Words to use** — approved terminology, brand language
- **Words to avoid** — banned terms, competitor names, off-brand phrases
- **Proof points** — key metrics, customer quotes, case study data
- **Content types** — what the site publishes (blog, landing pages, docs, social, email)
- **Visual style** — design constraints that affect copy (dark theme, minimal, data-heavy)

If no brand context is provided, ask for one before writing. Without it, copy will be generic and miss the brand's voice.

## Content Taxonomy

| Type | Purpose | Key Constraint |
|------|---------|----------------|
| Landing page | Convert visitors → action | One CTA per page, benefits over features |
| Blog post | Educate, build authority, drive organic traffic | Searchable: keyword-driven, structured for SEO |
| Case study | Prove value with real outcomes | Data-backed, customer-approved quotes |
| Documentation | Help users succeed | Scannable, task-oriented, no marketing tone |
| FAQ | Handle objections, reduce support load | Real questions from real users, concise answers |
| Email | Nurture leads, retain users | Subject line is 80% of the work, one action per email |
| Social post | Build audience, drive traffic, establish voice | Platform-specific format and tone |
| Ad copy | Capture attention, drive clicks | Character limits, clear value prop, strong CTA |
| Comparison page | Win competitive evaluations | Honest, specific, never disparage competitors |

## Module Catalog

| Module | Purpose | Key Frameworks |
|--------|---------|----------------|
| **copywriting** | Write new marketing copy | Headline formulas, CTA patterns, page-type guidance, voice handling |
| **copy-editing** | Review and polish existing copy | Nine Sweeps structured editing, quick-pass checks |
| **content-strategy** | Plan what to write and why | Searchable vs Shareable, content pillars, buyer-stage keywords |
| **social-content** | Create platform-specific social posts | Hook formulas, repurposing system, content calendar |
| **writing-quality** | Detect and eliminate AI writing patterns | 29-pattern detection, 5-dimension scoring, content-type-aware thresholds |

## Loading Order

1. This file (`_skill.md`) — pack overview, brand context protocol
2. Consumer's `brand-context.md` — site-specific voice and constraints
3. Role file (`_roles/writer.md` or `_roles/editor.md`) — task lifecycle
4. Module `_skill.md` — domain-specific methodology
5. Module `references/` — detailed frameworks, loaded on demand

## Cross-Module Dependencies

- `writing-quality` is referenced by both `copy-editing` (as the Anti-AI-Slop sweep) and by the `writer` role (as the final quality check before submission)
- `copywriting` and `copy-editing` are complementary: use copywriting to draft, copy-editing to review
- `content-strategy` informs what to write; `copywriting` and `social-content` handle the actual writing

## Workflow Catalog

| Workflow | Module | Purpose |
|----------|--------|---------|
| `writing_quality_check` | writing-quality | Score content on 5 dimensions, detect AI patterns, content-type-aware pass/fail verdict |

## Provenance & Confidence

**Overall: ~85% GEN / 15% RES** | **Pack total: ~3,700 lines across 22 files**

Only `writing-quality` has external source attribution. All other modules are generated domain expertise without cited research.

**Confidence rule:** 0.49 max. GEN-only modules set to 0.24 (at least 2x lower than RES-backed modules).

**Calibration status:** Scoring calibrated against 5 gold-standard landing pages (Stripe, Linear, Basecamp, Vercel, Notion). Content-type-aware thresholds: 28/50 for landing pages, 35/50 for prose.

### GEN / RES per Module

```
                         0%       25%       50%       75%      100%
                         │         │         │         │         │
writing-quality (0.49)   █████████████▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓  33% GEN / 67% RES
copywriting     (0.24)   ████████████████████████████████████████  100% GEN
copy-editing    (0.24)   ████████████████████████████████████████  100% GEN
content-strategy(0.24)   ████████████████████████████████████████  100% GEN
social-content  (0.24)   ████████████████████████████████████████  100% GEN
roles           (0.24)   ████████████████████████████████████████  100% GEN
                         │         │         │         │         │
                         ██ GEN (no source)    ▓▓ RES (cited)
```

### Source Attribution

| Source | License | Contributes to | Lines |
|--------|---------|---------------|-------|
| [blader/humanizer](https://github.com/blader/humanizer) | MIT | writing-quality: 29 AI patterns from Wikipedia WikiProject AI Cleanup | 217 |
| [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) | MIT | writing-quality: 5-dimension scoring, banned phrases/structures, 8 core rules | 373 |
| *(none)* | — | copywriting, copy-editing, content-strategy, social-content, roles | ~3,100 |

### Calibration Results

Tested against 5 recognized landing pages (v3, after content-type fixes):

| Site | Score | Verdict | Key finding |
|------|:-----:|---------|-------------|
| stripe.com | 27 | REVISE (-1) | Strong proof but generic promo language |
| linear.app | 22 | REVISE (-6) | Unsourced claims, repetitive positioning |
| basecamp.com | 26 | REVISE (-2) | Strongest voice, em-dash overuse |
| vercel.com | 25 | REVISE (-3) | Unattributed metrics |
| notion.com | 28 | PASS | Most named proof points |
| **example-saas.com** | 28 | PASS | Direct, specific, trust is the weak dimension |

## Later Improvements

### Calibration Gaps (from v3 testing)

**Scoring threshold:** 3 of 5 gold-standard sites fail by 1-3 points. Consider lowering landing-page threshold to 25 or adding a "borderline pass" band at 25-27.

**Trust dimension still too strict:** Even with the "specific and verifiable" adjustment, the tool flags named metrics (e.g., Stripe's "US$1.9tn") as needing inline sources. Consider a further exception: metrics attributed to the site's own company/product are inherently first-party and need no external citation.

**Negative calibration missing:** No test against known-bad copy to confirm the tool catches it. Add 2-3 generic AI SaaS pages as negative test cases (expected score <22).

### Research Targets (GEN → RES)

**High priority (100% GEN, high impact):**
- `copy-editing`: Nine Sweeps methodology — cite editing frameworks (Ann Handley *Everybody Writes*, Zinsser *On Writing Well*, AP Stylebook)
- `copywriting`: headline formulas — cite Copyhackers (Joanna Wiebe), published conversion research, Unbounce landing page studies
- `content-strategy`: prioritization scoring — cite HubSpot Content Strategy, Animalz research, Orbit Media annual blogging survey
- `writing-quality`: scoring.md rubric thresholds — anchor to published readability research (Flesch-Kincaid, Hemingway, Contently scoring)

**Medium priority:**
- `copywriting`: CTA copy guidelines — cite ConversionXL button copy studies, Unbounce CTA research
- `content-strategy`: topic cluster methodology — cite HubSpot pillar/cluster model documentation
- `social-content`: hook formulas — cite LinkedIn algorithm research (Richard van der Blom studies), Justin Welsh methodology
- `social-content`: platform algorithm behavior — cite platform documentation, Hootsuite/Buffer research reports

**Low priority (experiential, hard to cite):**
- All Tips and Gotchas sections — domain expertise by nature
- Roles (writer, editor) — process-oriented, not fact-oriented
- Plain English Alternatives reference — widely known substitutions, no single authoritative source
