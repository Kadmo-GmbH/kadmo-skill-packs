# Copy Frameworks

Headline formulas, page section templates, and page structure templates. Referenced by the copywriting `_skill.md`. Load on demand when drafting specific page types.

## Headline Formulas

Organized by the angle the headline takes. Each formula includes the pattern and 2-3 concrete examples. Mix angles across variants — don't write three headlines that all use the same formula.

### Outcome-Focused

Lead with the result the customer gets.

| Formula | Example |
|---------|---------|
| [Verb] [desirable outcome] without [common pain] | "Ship production code without waiting on DevOps" |
| [Verb] [desirable outcome] in [timeframe] | "Close deals in half the time" |
| [Desirable outcome], [mechanism] | "Faster deploys, zero manual steps" |
| [Desirable outcome] — starting [low-friction entry] | "Better hiring decisions — starting with your next role" |
| [Audience] [verb] [X]% [more/faster/better] with [product category] | "Engineering teams ship 40% faster with automated pipelines" |

**When to use:** When the outcome is concrete, measurable, and immediately desirable. Works best when the ICP has a clear goal they can articulate.

### Problem-Focused

Name the pain the visitor is experiencing.

| Formula | Example |
|---------|---------|
| Stop [pain point]. Start [desired state]. | "Stop chasing approvals. Start shipping." |
| [Pain point] is costing you [consequence] | "Manual deploys are costing you 8 hours every sprint" |
| You shouldn't need [painful workaround] to [desired outcome] | "You shouldn't need 3 tools and a spreadsheet to track deployments" |
| Tired of [pain]? | "Tired of debugging CI pipelines at midnight?" |
| [Pain point] solved. | "Flaky tests, solved." |

**When to use:** When the pain is acute, widely shared, and emotionally resonant. The visitor should read it and think "that's exactly my problem." Avoid if the pain is mild or if the audience doesn't yet recognize they have the problem.

### Audience-Focused

Speak directly to a specific group.

| Formula | Example |
|---------|---------|
| For [audience] who [situation] | "For engineering leads who spend more time in meetings than in code" |
| Built for [audience], not [anti-audience] | "Built for developers, not project managers" |
| The [product category] [audience] actually [verb] | "The deployment tool engineers actually want to use" |
| [Audience]: [direct statement] | "DevOps teams: your pipeline shouldn't need a pipeline" |

**When to use:** When the ICP is sharply defined and the audience identity is a source of pride or frustration. Strong for niche products. Weak for horizontal products that serve many audiences.

### Differentiation-Focused

Position against the category or competitors.

| Formula | Example |
|---------|---------|
| The only [category] that [unique benefit] | "The only CI platform that runs tests in parallel by default" |
| [Category] without [industry compromise] | "Monitoring without the dashboard fatigue" |
| Not another [category]. [Differentiation.] | "Not another project management tool. A system that ships projects." |
| [Competitor approach] is [problem]. [Your approach] is [benefit]. | "Dashboards show you what happened. Alerts tell you what to do." |

**When to use:** When the product has a genuine, defensible differentiator. Don't use if the differentiation is marginal or requires explanation. The visitor should immediately understand why this is different.

### Proof-Focused

Lead with evidence.

| Formula | Example |
|---------|---------|
| [Number] [people/companies] [action/result] | "2,400 teams deploy with confidence" |
| [Metric] [improvement] for [audience type] | "47% fewer production incidents for teams using automated rollbacks" |
| Trusted by [recognizable names] | "Trusted by Shopify, Stripe, and Linear" |
| [Customer name] [achieved result]. You can too. | "Vercel cut deploy times by 80%. Your team is next." |
| [Number] [things done] and counting | "14 million deployments and counting" |

**When to use:** When you have strong, specific proof. Vague proof ("trusted by thousands") is weaker than a specific number ("trusted by 2,400 teams"). Named logos are stronger than anonymous counts. Metrics with context are stronger than metrics alone.

## Page Section Templates

Templates for each recurring section type. Each includes purpose, structure, and copy guidance.

### Hero Section

**Purpose:** Convert attention into interest in under 5 seconds.

**Structure:**
- Headline (5-12 words)
- Subheadline (1-2 sentences, supports headline with specificity)
- Primary CTA button
- Optional: secondary CTA link, hero image/screenshot, social proof bar (logo strip or "trusted by X teams")

**Copy guidance:**
- Headline states the benefit. Subheadline adds the mechanism or the audience.
- If using a social proof bar, keep it to logos or a single metric. No full testimonials above the fold.
- CTA should be specific to the page's conversion goal. "Start free trial" not "Learn more."
- Avoid stacking too much copy above the fold. Headline + subheadline + CTA is usually enough. Let the page breathe.

### Social Proof Section

**Purpose:** Reduce skepticism by showing others trust and benefit.

**Structure (pick one or combine):**
- Logo strip (6-12 recognizable logos)
- Metric bar (2-4 aggregate stats: "2,400 teams", "14M deploys", "99.9% uptime")
- Testimonial cards (2-3 attributed quotes with name, title, company)
- Case study snippets (1-2 short summaries with key metric and link to full story)

**Copy guidance:**
- Attribution is everything. An unattributed quote is almost worthless. Include name, title, and company.
- Testimonials that include a specific metric ("cut deploy time by 80%") outperform general praise ("great product").
- If using logos, choose the most recognizable ones. Unknown logos don't build trust.
- If using metrics, make them concrete: "2,400 teams" beats "thousands of users."

### Features / Benefits Section

**Purpose:** Show what the product does by framing capabilities as outcomes.

**Structure:**
- Section headline (benefit-oriented, not "Features")
- 3-4 feature blocks, each containing:
  - Feature headline (state the benefit, not the feature name)
  - 1-2 sentence description (how it works, what changes)
  - Optional: icon, screenshot, or supporting metric

**Copy guidance:**
- Each feature headline should pass the "so what" test. "Automated pipelines" fails. "Deploy in 5 minutes, not 5 hours" passes.
- Descriptions should be concrete. What does this feature actually do for the customer, in specific terms?
- Three or four blocks is ideal. More than five overwhelms. Fewer than two feels thin.
- If a feature needs more than 2 sentences to explain, it might deserve its own feature page.

### How It Works Section

**Purpose:** Make the product feel achievable by showing the path from signup to outcome.

**Structure:**
- Section headline ("How it works" or a benefit-oriented alternative)
- 2-4 numbered steps, each containing:
  - Step number
  - Short title (3-5 words)
  - One sentence description

**Copy guidance:**
- Three steps is the sweet spot. Two feels too simple. Four is acceptable. Five or more defeats the purpose — the section is supposed to make things feel easy.
- Each step title should describe an action the customer takes, not a technical process that happens behind the scenes.
- The final step should be the outcome, not a product action. "Step 3: Your team ships faster" not "Step 3: The system processes your configuration."

### Pricing Section

**Purpose:** Help visitors self-select a plan with minimal friction.

**Structure:**
- Section headline
- 2-4 tier cards, each containing:
  - Tier name
  - Price (monthly/annual toggle if applicable)
  - 1-sentence description of who the tier is for
  - Feature list (5-8 items per tier)
  - CTA button
- Recommended tier highlighted visually
- Optional: enterprise callout, FAQ below

**Copy guidance:**
- Tier names should signal who the tier is for, not internal product labels. "Starter" is acceptable. "Professional" is better. "Plan A" is meaningless.
- The 1-sentence description under the tier name is underused. "For growing teams that need automation" helps the visitor self-select faster than scanning the feature list.
- Feature lists should describe capabilities the visitor understands, not internal feature names. "Unlimited team members" not "Multi-seat SSO provisioning."
- Highlight the tier you want most visitors to choose. This is typically the middle tier.

### FAQ Section

**Purpose:** Address remaining objections and reduce support load.

**Structure:**
- Section headline
- 6-10 question-answer pairs
- Accordion or visible list (both work)

**Copy guidance:**
- Questions should be phrased the way a real person would ask them. "What happens if I go over my limit?" not "What is the overage policy?"
- Answers should be 2-4 sentences. If an answer requires a paragraph, the question might deserve its own page.
- Front-load answers with the direct answer, then add context. "Yes, you can cancel anytime. No contracts, no cancellation fees."
- Use FAQ to handle pricing objections, security concerns, implementation questions, and integration compatibility.

### Final CTA Section

**Purpose:** Convert visitors who have read the full page.

**Structure:**
- Headline (reinforces core benefit or creates mild urgency)
- 1 sentence of supporting copy
- Primary CTA button
- Optional: secondary CTA or trust signal ("No credit card required", "Free 14-day trial")

**Copy guidance:**
- This is not a repeat of the hero. The visitor has now read the full pitch. The headline can be more specific or confident.
- Adding a friction-reducer next to the CTA ("No credit card required," "Set up in under 5 minutes") can lift conversion.
- Keep it short. This section should not introduce new information. It closes the deal on what's already been presented.

## Page Structure Templates

Recommended section order for common page types. These are starting points — adjust based on the specific product, audience, and available proof.

### SaaS Landing Page

| Order | Section | Notes |
|-------|---------|-------|
| 1 | Hero | Headline + subheadline + CTA. One offer, one conversion goal. |
| 2 | Social proof bar | Logo strip or metric bar. Establish credibility immediately. |
| 3 | Problem / Pain | Name the pain the visitor is experiencing. Earn the right to present the solution. |
| 4 | Solution / Benefits | 3-4 benefit blocks. Lead with outcomes, support with features. |
| 5 | How it works | 3 steps from signup to outcome. Make it feel achievable. |
| 6 | Testimonials | 2-3 attributed quotes with specific outcomes. |
| 7 | Objection handling | Address top 2-3 hesitations (price, complexity, switching cost). |
| 8 | FAQ | 6-8 questions. Handle remaining objections. |
| 9 | Final CTA | Reinforce benefit + primary CTA + friction reducer. |

### Product / Feature Page

| Order | Section | Notes |
|-------|---------|-------|
| 1 | Hero | Feature benefit as headline. Screenshot or demo visual. |
| 2 | Problem context | What's broken without this feature? Keep it to 2-3 sentences. |
| 3 | Feature detail | Deep dive: how it works, what it does, screenshots or GIFs. |
| 4 | Benefits | 3-4 outcomes this feature enables. |
| 5 | Integration / compatibility | How this fits into the customer's existing stack. |
| 6 | Testimonial | 1-2 quotes specifically about this feature. |
| 7 | Final CTA | Link to trial, demo, or parent product page. |

### Comparison Page

| Order | Section | Notes |
|-------|---------|-------|
| 1 | Hero | "[Your Product] vs [Competitor]" headline. Neutral, not combative. |
| 2 | Summary table | Side-by-side comparison on 5-8 criteria. Checkmarks and specifics. |
| 3 | Key differences | 3-4 sections, each covering one differentiator in depth. |
| 4 | Where [competitor] wins | Optional but powerful. Honesty builds trust. |
| 5 | Migration / switching | How easy it is to switch. Address switching cost objection directly. |
| 6 | Testimonial | Quote from a customer who switched from the competitor. |
| 7 | Final CTA | "Try [product] free" or "See the difference." |

### About Page

| Order | Section | Notes |
|-------|---------|-------|
| 1 | Mission / story | Why the company exists. What problem the founders saw. Keep it genuine. |
| 2 | Team | Photos, names, roles. Optional: one line of personality per person. |
| 3 | Values | 3-5 operating principles. Specific and actionable, not generic platitudes. |
| 4 | Traction / milestones | Key metrics, funding, launch dates, customer count. Proof the company is real. |
| 5 | Final CTA | Careers link, contact, or product CTA depending on primary audience. |
