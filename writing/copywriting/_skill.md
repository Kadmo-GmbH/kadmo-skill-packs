---
name: Conversion Copywriting
domain: writing
parent: writing
tags: ["@copywriting", "@conversion", "@marketing"]
confidence: 0.24
---

# Copywriting

Conversion copywriting for marketing websites. Covers homepage heroes, landing pages, pricing pages, feature pages, comparison pages, and all supporting sections (social proof, FAQ, objection handling, CTAs). Produces draft copy ready for the copy-editing module to review and the writing-quality module to score.

## Brand Context

Read `brand-context.md` before writing any copy. Every brand decision depends on it.

If no brand context file exists, ask for these before drafting:

| Input | Why it matters |
|-------|---------------|
| **ICP (Ideal Customer Profile)** | Determines language, pain points, sophistication level |
| **Value proposition** | The core promise. Everything on the page supports this. |
| **Voice rules** | Tone, formality, personality traits. Without these, copy will be generic. |
| **Proof points** | Metrics, customer logos, testimonials. Proof is what separates claims from credibility. |
| **Competitor differentiation** | What you do that they don't. Avoids writing generic category copy. |
| **Words to avoid** | Competitor names, banned terms, off-brand language |

Without brand context, copy defaults to clear and competent but personality-less. That is not good enough for production.

## Copywriting Principles

Six principles, ranked by priority. When they conflict, the higher-ranked principle wins.

1. **Clarity over cleverness** — If a headline requires a second read to understand, it fails. Clever wordplay that obscures meaning costs conversions. Write so the visitor gets it in under 3 seconds.

2. **Benefits over features** — Features describe what the product does. Benefits describe what changes for the customer. "AI-powered workflow engine" is a feature. "Ship code 2x faster without hiring" is a benefit. Lead with the benefit, support with the feature.

3. **Specificity over vagueness** — "Trusted by thousands" is vague. "Used by 2,400 engineering teams" is specific. Specific claims build trust. Vague claims trigger skepticism. Use real numbers, real names, real outcomes.

4. **Customer language over company jargon** — Write in the words your ICP uses, not the words your product team invented. If customers say "automated testing" and the product calls it "quality assurance orchestration," use "automated testing."

5. **One idea per section** — Each section on the page has one job. The hero converts attention into interest. Social proof converts doubt into trust. The CTA converts interest into action. When a section tries to do two jobs, it does neither well.

6. **Proof over promises** — Any company can claim they are "the leading platform." Proof makes claims believable. Metrics, customer quotes, case studies, logos, integrations, and usage data are all forms of proof. Use them everywhere.

## Writing Style Rules

Rules applied at the sentence level, across all copy.

| Prefer | Over | Why |
|--------|------|-----|
| Simple words | Complex words | "Use" beats "utilize." "Help" beats "facilitate." Simpler words are faster to process. |
| Specific claims | Vague claims | "Reduces deploy time by 40%" beats "significantly improves deployment speed." |
| Active voice | Passive voice | "Teams ship faster" beats "faster shipping is achieved." Active voice names the actor. |
| Confident tone | Qualified tone | "This works" beats "this can potentially help." Hedging signals uncertainty. |
| Showing | Telling | A customer quote showing the outcome beats a paragraph claiming the outcome. |
| Honest framing | Sensational framing | "Saves 6 hours per sprint" beats "revolutionary game-changing paradigm shift." Overselling erodes trust. |

Additional rules:

- **Sentence length:** Vary it. Mix 5-word punches with 20-word explanations. Uniform length reads robotic.
- **Paragraphs:** Keep them to 1-3 sentences for web copy. Dense paragraphs kill scannability.
- **Jargon:** Use it only when the ICP uses it. If writing for developers, "CI/CD pipeline" is fine. If writing for executives, it is not.
- **Pronouns:** "You" and "your" for the customer. "We" for the brand (sparingly). Never "one" or "users."
- **Numbers:** Use digits, not words. "3 steps" is faster to scan than "three steps." Exception: "one" as a pronoun or emphasis ("one platform").
- **Superlatives:** Avoid "best," "fastest," "most powerful" unless backed by verifiable proof. Unsubstantiated superlatives are legally risky and erode credibility.
- **Negative framing:** Use sparingly and only in problem sections. Solution sections should be positive. "Stop wasting time" works in a pain section. The CTA should be "Start saving time," not "Stop wasting time."

### Before / After Examples

| Weak | Strong | Problem fixed |
|------|--------|---------------|
| "Our platform leverages cutting-edge AI to streamline your workflow." | "Ship code 2x faster. AI handles the pipeline." | Jargon, passive, vague |
| "We offer a comprehensive suite of tools designed for modern teams." | "Everything your team needs to deploy, monitor, and debug — in one place." | Generic, tells instead of shows |
| "Significantly improve your development velocity." | "Teams using [product] merge 47% more PRs per sprint." | Vague claim replaced with specific proof |
| "Get started today and see the difference." | "Start your free trial — deploy your first project in 5 minutes." | Generic CTA replaced with specific outcome |
| "Trusted by industry leaders worldwide." | "Used by 2,400 engineering teams, including Shopify, Linear, and Vercel." | Vague social proof replaced with specifics |

## Above the Fold

The hero section is the highest-leverage copy on the page. Visitors decide in under 5 seconds whether to stay or leave. The hero must do three things:

1. **Headline** — State the core benefit or outcome. Not a description of the product. Not a category label. The headline answers: "What changes for me if I use this?"
2. **Subheadline** — Support the headline with specificity. Add the "how" or the "for whom." Keep it to 1-2 sentences.
3. **CTA** — One primary action. Clear, specific, low-friction.

### Headline Formulas (Summary)

| Type | Pattern |
|------|---------|
| Outcome-focused | [Verb] [desirable outcome] without [common pain] |
| Problem-focused | Stop [pain]. Start [desired state]. |
| Audience-focused | For [audience] who [situation] |
| Differentiation | The only [category] that [unique benefit] |
| Proof-focused | [Number] [people/companies] [action/result] |

Full formula catalog with examples: `references/copy-frameworks.md`

### Hero Copy Checklist

- Does the headline communicate a clear benefit in under 10 words?
- Does the subheadline add specificity the headline lacks?
- Is the CTA a specific action, not a vague label?
- Would a visitor who knows nothing about the product understand the value in 5 seconds?
- Is there any jargon that excludes part of the target audience?
- Does the headline work on mobile (under 8 words for single-line display at 320px)?
- Is the subheadline doing real work, or just restating the headline?

### Common Hero Mistakes

- **Category label as headline:** "The Modern Deployment Platform" tells the visitor what the product is, not what it does for them. Category labels belong in the subheadline or navigation, not the headline.
- **Multiple benefits crammed in:** "Deploy faster, monitor better, debug easier, and collaborate seamlessly" is four headlines fighting for attention. Pick the strongest one.
- **Weak subheadline:** "Learn more about how we can help your team" adds nothing. The subheadline should add specificity: the audience, the mechanism, or a supporting proof point.
- **No CTA above the fold:** If the visitor has to scroll to find the action, you lose the ones who were ready to convert immediately.

## CTA Copy Guidelines

Weak CTAs are vague, passive, or generic. Strong CTAs tell the visitor exactly what they will get.

### Weak CTA Examples

| CTA | Problem |
|-----|---------|
| "Submit" | Says nothing about what happens next |
| "Learn More" | Vague. Learn more about what? |
| "Click Here" | Describes the mechanic, not the outcome |
| "Get Started" | Acceptable but generic. Started with what? |
| "Contact Us" | Puts burden on the visitor with no promise of value |

### Strong CTA Formula

**[Action Verb] + [What They Get] + [Qualifier]**

The action verb is what they do. What they get is the outcome. The qualifier reduces friction or adds urgency.

| CTA | Structure |
|-----|-----------|
| "Start your free trial" | Start + free trial + (implied: no commitment) |
| "See it in action — 2 min demo" | See + demo + time qualifier |
| "Get your report — free" | Get + report + free |
| "Deploy in 5 minutes" | Deploy + (implied: your project) + time qualifier |
| "Book a 15-minute walkthrough" | Book + walkthrough + time qualifier |

### CTA Placement Rules

- One primary CTA per page. Repeat it, but don't compete with it.
- Place the primary CTA above the fold and after the final section.
- Secondary CTAs (e.g., "Watch demo" alongside "Start free trial") are fine if they serve visitors at different commitment levels.
- Never put two CTAs of equal visual weight next to each other. One must be primary, one secondary.

## Voice and Tone

Voice is the brand's personality. Tone is how that personality adapts to context.

### Calibrating from Brand Context

The brand context file specifies voice attributes. Map them to concrete writing decisions:

| Voice Attribute | Writing Decision |
|-----------------|-----------------|
| Casual | Contractions, sentence fragments, first person |
| Professional | Full sentences, moderate formality, no slang |
| Formal | No contractions, third person, complete sentences |
| Technical | Domain jargon allowed, precision over simplicity |
| Playful | Wordplay acceptable (when clear), personality encouraged |
| Authoritative | Data-backed claims, confident assertions, no hedging |

### Formality Levels

| Level | Characteristics | Example |
|-------|----------------|---------|
| **Casual** | Contractions, fragments, humor | "Build faster. Ship more. Stress less." |
| **Conversational** | Contractions, full sentences, warm | "You spend too much time on deploys. We fix that." |
| **Professional** | No contractions, clear, direct | "Reduce deployment time by 40% with automated pipelines." |
| **Formal** | No contractions, precise, measured | "The platform reduces mean deployment time by 40% through automated pipeline management." |

### Consistency Rules

- Voice stays consistent across the entire site. A casual homepage and formal pricing page breaks trust.
- Tone can shift by page type: landing pages can be more energetic, documentation more measured, error messages more empathetic.
- When in doubt, match the voice of the brand context. When there is no brand context, default to conversational.

### Voice Anti-Patterns

| Anti-pattern | What it sounds like | Fix |
|--------------|---------------------|-----|
| **Corporate nothing** | "We are committed to delivering best-in-class solutions." | State what you actually do, in plain language. |
| **Startup hype** | "We're on a mission to revolutionize how the world deploys code." | Drop the grandiosity. State the benefit. |
| **Fake casual** | "So, like, we built this thing? And it's pretty great?" | Casual doesn't mean sloppy. Contractions yes, filler words no. |
| **AI generic** | "In today's rapidly evolving digital landscape..." | Delete and start with the actual point. |
| **Feature-first** | "Our platform features an AI-powered orchestration engine with..." | Lead with what this means for the customer, not what it is. |

## Page-Type Guidance

| Page Type | Primary Goal | Key Copy Principle | Typical Length |
|-----------|--------------|--------------------|----------------|
| **Homepage** | Communicate value, route visitors deeper | Breadth over depth. Cover the full value prop, link to detail pages. | 5-8 sections |
| **Landing Page** | Convert on one specific offer | Depth over breadth. One offer, one CTA, every section supports conversion. | 6-10 sections |
| **Pricing Page** | Help visitors self-select a plan | Clarity and comparison. Show what's included at each tier. Reduce decision anxiety. | 3-5 sections |
| **Feature Page** | Explain one capability in depth | Benefit-first, feature-second. Show the outcome, then explain the mechanism. | 4-7 sections |
| **About Page** | Build trust and human connection | Story over claims. Who built this, why, and what they believe. | 3-5 sections |
| **Comparison Page** | Win competitive evaluations | Honest specificity. Compare on real criteria. Never disparage competitors. | 4-6 sections |

Recommended section orders for each page type: `references/copy-frameworks.md`

## Copy Diagnosis

When reviewing existing copy (yours or someone else's), diagnose before rewriting. These are the five most common failure modes:

| Failure Mode | Symptom | Fix |
|--------------|---------|-----|
| **No clear value prop** | Reader finishes the page and can't articulate what the product does for them | Rewrite the hero to lead with the primary benefit |
| **Feature dump** | Page lists 10+ features with no hierarchy or connection to outcomes | Group features under 3-4 benefit themes, cut the rest |
| **Missing proof** | Claims without evidence ("industry-leading," "trusted by thousands") | Replace vague claims with specific metrics, quotes, or logos |
| **Wrong audience** | Copy uses language the ICP doesn't use, or addresses pains they don't have | Interview customers (or read their reviews) and rewrite in their language |
| **Conversion friction** | CTA is buried, vague, or asks for too much commitment too early | Simplify CTA, reduce form fields, add friction reducers ("No credit card required") |

When diagnosing, read the page as three different visitors: (1) someone who has never heard of the product, (2) someone evaluating it against a competitor, and (3) someone ready to buy but looking for a reason to hesitate. Each perspective reveals different weaknesses.

## Core Section Types

Every marketing page is assembled from a set of recurring section types. Know what each one does.

### Social Proof
Reduces skepticism by showing that others have already trusted and benefited. Forms: customer logos, testimonial quotes, case study summaries, aggregate metrics ("2,400 teams"), awards, press mentions. Place early on the page, ideally right after the hero. Proof should be specific and attributable — "Great product!" from an unnamed person is worthless.

### Problem / Pain
Names the problem the visitor is experiencing. This section earns the right to present your solution by first demonstrating that you understand the problem. Use the customer's language for the pain. Be specific about consequences: not "inefficient workflows" but "your team spends 8 hours a week on manual deploys." If the visitor doesn't recognize themselves in this section, the solution section won't land.

### Solution / Benefits
Presents your product as the answer to the problem. Lead with outcomes (what changes for the customer), not mechanics (how the product works). Each benefit should map to a pain point named earlier. Format as 3-4 benefit blocks, each with a headline, 1-2 sentence description, and optional supporting visual or metric.

### How It Works
Reduces complexity anxiety by showing the path from signup to outcome. Three steps is the standard (it can be two or four, but never six). Each step: number, short title, one sentence. The goal is to make the product feel simple and achievable, even if the underlying technology is complex.

### Objection Handling
Anticipates and addresses the reasons visitors hesitate. Common objections: price, complexity, switching costs, security, support. Can be woven into other sections or presented as a standalone section. FAQ format works well for this. Address objections with proof, not reassurance. "We take security seriously" is reassurance. "SOC 2 Type II certified, annual pen tests, 99.99% uptime SLA" is proof.

### FAQ
Answers real questions from real users. Source questions from sales calls, support tickets, and search queries — not from a brainstorming session. Keep answers concise (2-4 sentences). Use FAQ to handle remaining objections that don't fit elsewhere. Never pad FAQ with questions nobody asks.

### Testimonials
A specific form of social proof using direct customer quotes. Strong testimonials include: the customer's name and title, their company, the specific outcome they achieved, and ideally a metric. "It just works great!" is a weak testimonial. "We cut our deploy time from 45 minutes to 6 minutes. Our team ships 3x more per sprint." is a strong one. Always attribute to a real person.

### Final CTA
The closing conversion section. Appears at the bottom of the page. Repeats the primary CTA from the hero but with added context: by this point, the visitor has read the full pitch. The headline here should reinforce the core benefit or create mild urgency. Keep it short — headline, 1 sentence, CTA button.

### Section Copy Length Guidelines

Getting length right matters as much as getting the words right. Too long and visitors skip. Too short and the section feels empty.

| Section | Headline | Body copy | Notes |
|---------|----------|-----------|-------|
| Hero | 5-12 words | 15-30 words (subheadline) | Shorter is almost always better |
| Social proof | 3-8 words | Varies by format | Logos need no body copy. Testimonials: 2-3 sentences max. |
| Problem / Pain | 5-10 words | 30-60 words | Be specific and concise. Over-describing the pain becomes whining. |
| Solution / Benefits | 5-10 words per block | 15-30 words per block | 3-4 blocks. Each block is a mini-argument. |
| How it works | 3-5 words per step | 10-20 words per step | Brevity is the point. If a step needs a paragraph, the process is too complex. |
| FAQ | Real question phrasing | 20-60 words per answer | Front-load the direct answer. Context comes second. |
| Final CTA | 5-10 words | 10-20 words | Shortest section on the page. Everything has already been said. |

## Common Tasks

1. **Write homepage hero** — Draft headline, subheadline, and CTA for a homepage. Requires brand context. Deliver 3 variants with different angles (outcome, problem, audience).

2. **Write landing page** — Draft full page copy for a specific offer or campaign. Define section order, write all sections, include CTA placement. Follow the landing page template in `references/copy-frameworks.md`.

3. **Write pricing page** — Draft tier names, descriptions, feature lists, and CTA copy for each tier. Include a FAQ section addressing pricing objections. Highlight the recommended tier.

4. **Write CTA variants** — Generate 5-8 CTA options for a given conversion goal. Include primary and secondary CTA pairs. Test different angles: outcome, urgency, low-friction, social proof.

5. **Rewrite weak copy** — Take existing copy that underperforms and improve it. Diagnose the problem first (vague headline? missing proof? wrong voice?), then rewrite with specific improvements noted. Explain what changed and why.

6. **Write feature section** — Draft a benefit-led section for a single product feature. Headline (benefit), description (how it works), supporting proof (metric or quote).

7. **Write social proof section** — Assemble a social proof block from available proof points. Select the right format (logos, quotes, metrics, case study snippets) based on what's available.

8. **Write comparison page** — Draft an honest comparison against a named competitor. Compare on specific, verifiable criteria. Acknowledge competitor strengths. Win on differentiation, not disparagement.

9. **Write FAQ section** — Draft 6-10 FAQ items for a given page. Source questions from real user concerns (provided in brand context or brief). Keep answers tight.

10. **Write objection-handling copy** — Identify the top 3-5 objections for a given product/offer and draft copy that addresses each with proof.

### Task Execution Pattern

For any copywriting task, follow this sequence:

1. **Load brand context** — Read `brand-context.md`. If it doesn't exist, request the minimum inputs (ICP, value prop, voice, proof points) before proceeding.
2. **Identify the page type** — Determine which page structure template to use from `references/copy-frameworks.md`.
3. **Define the conversion goal** — What specific action should the visitor take? This drives every section.
4. **Draft the section order** — Choose which core section types the page needs, in what order.
5. **Write body sections first, hero last** — The strongest line in the body copy often becomes the headline.
6. **Run the delivery checklist** — Every item must pass before the copy is delivered.
7. **Send to copy-editing** — The copy-editing module applies structured review. Don't self-edit; use the module.
8. **Run writing-quality check** — The writing-quality module scores the final copy. Must pass 35/50.

## Tips

- Write the headline last. Write the full page first, find the strongest line, and promote it to the headline. The first headline you write is almost never the best one.
- Delete your first paragraph. The real opening is usually buried in paragraph two or three. First paragraphs tend to be throat-clearing.
- Read your copy as if you are the most skeptical visitor. At every claim, ask "prove it." If you cannot, cut the claim or add proof.
- Use the customer's words, literally. Pull phrases from reviews, support tickets, sales calls. When customers describe the problem in their own words, that language converts better than anything you write from scratch.
- One page, one reader, one action. If you catch yourself writing "whether you're a startup or enterprise," you are writing for two audiences. Pick one or make two pages.
- Subheadlines do more work than headlines on long pages. Visitors scan subheads to decide what to read. Make each subhead a standalone benefit statement.
- After writing, count your proof points. If a section makes a claim with zero proof, either find proof or rewrite the claim as something provable.
- Short copy is not automatically better than long copy. Long copy that maintains interest outperforms short copy that leaves questions unanswered. The goal is "no unnecessary words," not "few words."
- The "so what" test: After every sentence, ask "so what?" If the sentence doesn't survive that question, either make it specific or cut it.
- Write three headline variants using three different formulas (outcome, pain, proof). This forces genuine variation. Three rephrasings of the same angle are not variants.
- Every section should pass the "screenshot test" — if you took a screenshot of just that section with no surrounding context, would a visitor understand the value being communicated?
- When writing for a technical audience, precision matters more than simplicity. Developers will dismiss copy that oversimplifies their domain. "Automates your CI/CD pipeline" is better than "makes deployment easy" when writing for engineers.
- Anchor pricing copy to value, not cost. "For $49/month, you get..." frames the price as an expense. "$49/month saves your team 20 hours of manual work" frames it as an investment.

## Gotchas

- **Generic copy trap:** Without brand context, all copy tends toward the same SaaS cliches ("streamline your workflow," "all-in-one platform," "built for modern teams"). If you catch yourself writing these, stop and ask for more brand context.
- **Feature obsession:** Product teams want every feature mentioned. Resist. A page that lists 20 features converts worse than one that demonstrates 3 benefits. Prioritize by ICP pain points.
- **Voice inconsistency:** The hero can't be casual and punchy if the rest of the page is formal and measured. Write the hero first to set the voice, then match it throughout.
- **Proof-free zones:** Sections without any form of proof (metric, quote, logo, screenshot) feel like empty claims. Every section should include at least one proof element.
- **CTA mismatch:** If the page sells a free trial, the CTA should say "Start free trial," not "Get in touch." Match the CTA to the actual next step.
- **Headline overload:** Writing 3 headline variants that all say the same thing in slightly different words is not useful variation. Each variant should take a different angle (outcome vs. pain vs. audience vs. proof).
- **Ignoring mobile:** Over half of web traffic is mobile. If your headline runs to 15 words, it wraps to 4 lines on mobile and loses all punch. Test at 320px width mentally.
- **Over-optimizing for cleverness:** A pun or metaphor in the headline that requires context the visitor doesn't have will lower conversion. When in doubt, choose the clearer version.
- **Testimonial without attribution:** An unattributed quote reads as fabricated even when it is real. Always include name, title, and company. If the customer won't allow attribution, use a case study format instead.
- **Pricing page without FAQ:** Pricing pages have the highest objection density of any page type. Always include a FAQ section that addresses billing, refunds, plan changes, and "what happens if I go over my limit" questions.
- **Same CTA everywhere:** If the hero says "Start free trial," the features section says "Request a demo," and the footer says "Contact sales," the visitor faces three different conversion paths. Pick one primary action and reinforce it. Other actions can exist as secondary links, not competing CTAs.
- **Skipping the problem section:** Jumping straight to the solution without naming the problem assumes the visitor already knows they need your product. They usually don't. The problem section earns the right to present the solution.
- **Writing in a vacuum:** Copy written without looking at the actual page design often misses layout constraints. A 40-word headline that looks fine in a doc breaks a hero section. Always consider the visual context.

## Delivery Checklist

Run this checklist before delivering any page copy. Every item must pass.

1. **Brand context loaded?** — Copy reflects the brand's voice, not generic SaaS tone.
2. **Headline passes 5-second test?** — A stranger can understand the value prop in one read.
3. **Every section has proof?** — No claim without supporting evidence (metric, quote, logo, screenshot).
4. **One primary CTA?** — Repeated but not competing with alternatives.
5. **CTA matches the offer?** — "Start free trial" on a free-trial page, not "Contact us."
6. **Mobile-friendly lengths?** — Headlines under 8 words, subheadlines under 20 words.
7. **Customer language?** — Copy uses words the ICP uses, not internal product terminology.
8. **No AI tells?** — Run through the writing-quality module's pattern detection, or at minimum check for the banned phrases and structures.
9. **Voice consistent?** — Hero, body, and CTA all sound like they were written by the same person.
10. **Transitions clean?** — No "let's dive in," "but that's not all," or other AI-sounding connectors. See `references/natural-transitions.md`.
11. **Page flows logically?** — Each section answers the question the previous section raised.
12. **Scannable?** — A visitor who only reads headlines and subheadlines gets the full pitch.

## References

- `references/copy-frameworks.md` — headline formulas with examples, page section templates, recommended page structures for each page type
- `references/natural-transitions.md` — section-to-section transition phrases, AI-sounding transitions to avoid, flow principles

## Related Modules

- **copy-editing** — structured review methodology for polishing drafted copy. Use after copywriting to refine.
- **writing-quality** — AI pattern detection and 5-dimension scoring. Use as final quality gate before delivery.
- **content-strategy** — topic selection, keyword mapping, content pillar planning. Use before copywriting to decide what to write and why.
