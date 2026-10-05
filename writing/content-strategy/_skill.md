---
name: Content Strategy
domain: writing
parent: writing
tags: ["@strategy", "@planning", "@content"]
confidence: 0.24
---

# Content Strategy

Decides what to write, when to publish it, and why it matters. This module sits upstream of copywriting and social-content: it produces the plan they execute against. Without a strategy, you produce content that nobody searches for, nobody shares, and nobody remembers.

## Searchable vs Shareable

Every piece of content falls into one of two modes. Confusing them is the most common strategy mistake.

### Searchable Content

Captures **existing demand**. People already have questions; you answer them better than anyone else.

- Driven by keyword research, not inspiration
- Structured for SEO: clear headings, answer-first format, internal links
- Compounds over time (evergreen traffic curve)
- Examples: "how to set up CI/CD", "best project management tools 2025", "React vs Vue comparison"
- Success metric: organic search traffic, ranking position

### Shareable Content

Creates **new demand**. Nobody was searching for your insight because it didn't exist until you published it.

- Driven by original thinking: novel data, contrarian takes, proprietary research, lived experience
- Structured for engagement: strong hooks, narrative tension, quotable lines
- Spikes on publish, decays unless redistributed
- Examples: "We analyzed 10,000 deploys and here's what broke", "Why we stopped doing sprint planning", "The hidden cost of microservices nobody talks about"
- Success metric: social shares, backlinks, email signups, brand recall

### Which to Prioritize

**New sites: searchable first.** You need a baseline of organic traffic before shareable content has distribution. A brilliant essay with zero audience stays invisible.

**Established sites: shift toward shareable.** Once organic traffic covers your base, shareable content builds brand and differentiation. The 60/40 split (searchable/shareable) works for most teams. Adjust based on your distribution channels.

**The overlap zone.** The highest-leverage content is both: answers a real search query AND contains original insight that makes people share it. Target this when you can, but don't force it. Most content is clearly one or the other.

## Content Pillars

Pillars are the 3-5 core topics your site will own. Everything you publish should trace back to a pillar. If it doesn't, you're diluting focus.

### How to Identify Pillars

**Product-led.** What problems does your product solve? Each core problem area is a candidate pillar.

- Map each product feature to the problem it addresses
- Group related problems into themes
- Pick the themes where you have genuine expertise and competitive advantage

**Audience-led.** What does your target audience care about beyond your product?

- Interview 10 customers about their daily challenges
- Look at what they read, follow, and share
- Identify the professional identity they aspire to
- Pillars should connect their aspirations to your product's domain

**Search-led.** Where is there organic demand you can realistically capture?

- Pull keyword clusters from your domain using Ahrefs, SEMrush, or Google Search Console
- Filter for topics with reasonable volume (100+ monthly searches) and achievable difficulty
- Group related keywords into theme clusters
- Pick clusters where you can produce genuinely better content than page-one incumbents

**Competitor-led.** Where are competitors weak or absent?

- Audit competitor blogs: what topics do they cover well? Where are the gaps?
- Look for topics with audience demand but poor existing coverage
- Avoid pillars where a dominant player already owns the conversation unless you have a genuinely different angle

### Pillar Validation Criteria

A good pillar passes all four tests:

1. **Relevance** — directly connected to your product or audience needs
2. **Depth** — can support 15+ articles without stretching
3. **Expertise** — your team has genuine knowledge or experience here
4. **Demand** — people actually search for or care about this topic

## Topic Clusters

Once you have pillars, organize content into hub-and-spoke clusters. This is how search engines understand topical authority.

### Structure

```
                 ┌─────────────────┐
                 │   Pillar Page   │
                 │  (Comprehensive │
                 │   overview)     │
                 └────────┬────────┘
                          │
          ┌───────────────┼───────────────┐
          │               │               │
    ┌─────┴─────┐   ┌─────┴─────┐   ┌─────┴─────┐
    │ Subtopic  │   │ Subtopic  │   │ Subtopic  │
    │ Article A │   │ Article B │   │ Article C │
    └─────┬─────┘   └─────┬─────┘   └─────┬─────┘
          │               │               │
     ┌────┴────┐     ┌────┴────┐     ┌────┴────┐
     │Sub-sub A│     │Sub-sub B│     │Sub-sub C│
     └─────────┘     └─────────┘     └─────────┘
```

**Pillar page.** Comprehensive overview of the topic (2,000-4,000 words). Links to every subtopic article. Targets the broad head keyword.

**Subtopic articles.** Focused deep-dives on specific aspects (1,000-2,500 words). Each targets a long-tail keyword cluster. Links back to the pillar page and cross-links to related subtopics.

**Internal linking rules:**
- Every subtopic links back to its pillar page
- Pillar page links to every subtopic
- Related subtopics cross-link to each other
- Use descriptive anchor text (not "click here" or "read more")
- Place the most important link in the first two paragraphs

### Cluster Planning Template

For each pillar, map out:

1. Pillar page topic and target keyword
2. 8-15 subtopic articles with target keywords
3. Internal linking plan (which articles link to which)
4. Publishing sequence (pillar page first, then subtopics in priority order)
5. Content gaps vs competitors in this cluster

## Keyword Research by Buyer Stage

Different content serves different stages of the buyer journey. Map keywords to stages so your content portfolio covers the full funnel.

| Stage | What They Need | Modifier Formulas | Example Keywords |
|-------|---------------|-------------------|-----------------|
| **Awareness** | Understand their problem exists | "what is [topic]", "why does [problem] happen", "[symptom] causes", "guide to [concept]" | "what is CI/CD", "why do deployments fail", "slow website causes" |
| **Consideration** | Evaluate solution categories | "how to [solve problem]", "best [solution type]", "[option A] vs [option B]", "[solution type] comparison" | "how to automate testing", "best CI/CD tools", "Jenkins vs GitHub Actions" |
| **Decision** | Choose a specific solution | "[product] review", "[product] pricing", "[product] alternatives", "[product] vs [competitor]" | "CircleCI review 2025", "GitHub Actions pricing", "CircleCI vs GitLab CI" |
| **Implementation** | Succeed with chosen solution | "[product] setup guide", "[product] tutorial", "[product] best practices", "how to [specific task] with [product]" | "GitHub Actions setup guide", "CircleCI Docker tutorial", "CI/CD pipeline best practices" |

### Allocation by stage

Early-stage companies: weight toward Awareness and Consideration (70%). You need to build the audience before you can convert them.

Established companies: weight toward Decision and Implementation (60%). Your audience already knows you; help them buy and succeed.

## Content Ideation Sources

Where to find topics that your audience actually cares about. Use multiple sources to avoid echo-chamber planning.

**Keyword data.** Pull search queries from Google Search Console, Ahrefs, or SEMrush. Filter for relevant topics with volume and achievable difficulty. This is your most reliable source for searchable content.

**Sales and support conversations.** Record the questions prospects ask on sales calls and the issues customers raise in support tickets. Group by theme. Each recurring question is a content opportunity. This source produces content that directly addresses objections and reduces support load.

**Customer surveys.** Ask customers: "What's the hardest part of [your domain]?" and "What would you search for when looking for a product like ours?" Short, 3-question surveys get higher response rates than long ones.

**Community forums and Q&A sites.** Monitor Reddit, Stack Overflow, Quora, and industry-specific forums for questions in your domain. Look for questions with many upvotes but unsatisfying answers. These are gaps you can fill.

**Competitor analysis.** Audit competitor blogs for their most-linked and most-shared content. Use Ahrefs Content Explorer or BuzzSumo. Look for angles they missed or topics they covered superficially.

**Industry news and trends.** Follow industry publications, newsletters, and conference talks. React quickly to new developments with informed analysis. Shareable content often comes from being among the first to explain what a new development means.

**Internal expertise.** Interview engineers, designers, product managers, and executives at your company. They have insights from building and running the product that no external writer can replicate. Structure these as "lessons learned" or "how we built X" posts.

**Social listening.** Track mentions of your product, competitors, and industry keywords on social platforms. Notice what generates discussion, disagreement, or confusion. These signals point to content opportunities.

## Content Prioritization

Not every idea is worth executing. Score ideas on four factors to decide what to write next.

### Scoring Framework

| Factor | Weight | 1 (Low) | 5 (High) |
|--------|--------|---------|----------|
| **Customer Impact** | 40% | Tangential to audience needs | Directly addresses a top pain point or frequent question |
| **Content-Market Fit** | 30% | Generic take, many existing articles say the same thing | Unique angle, proprietary data, or genuine expertise advantage |
| **Search Potential** | 20% | No search volume, not shareable | High-volume keyword with achievable difficulty, or high share potential |
| **Resource Requirements** | 10% | Needs original research, expert interviews, custom graphics | Can be written with existing knowledge and assets |

**How to score:**

1. List all content ideas in a spreadsheet
2. Score each idea 1-5 on each factor
3. Multiply by weights: `(Customer Impact x 0.4) + (Content-Market Fit x 0.3) + (Search Potential x 0.2) + (Resource Requirements x 0.1)`
4. Sort by weighted score
5. Take the top 10. Sanity-check them: does this list feel right? If a high-scorer feels wrong, examine why. Quantitative scoring is a starting point, not a verdict.

**Resource Requirements scoring is inverted:** a 5 means "easy to produce," not "requires massive resources." This keeps the math simple (higher = better across all factors).

## Content Audit Framework

Before planning new content, audit what already exists. Most sites have valuable content buried under outdated articles, duplicate topics, and broken internal links.

### Audit Process

1. **Inventory.** Export all published URLs with titles, publish dates, word counts, and assigned pillars. Use a crawl tool (Screaming Frog, Sitebulb) or export from your CMS.

2. **Traffic analysis.** Pull 6-month traffic data from Google Analytics or Search Console. Tag each page: Growing (traffic trending up), Stable (flat), Declining (traffic trending down), Dead (fewer than 10 visits/month).

3. **Content quality review.** For each page, score on three criteria:
   - Accuracy: Is the information still correct? Are screenshots and links current?
   - Completeness: Does it fully cover the topic, or are there gaps a competitor fills?
   - Voice: Does it match your current brand voice and quality standards?

4. **Action assignment.** Based on traffic and quality, assign each page one of four actions:

| Traffic | Quality | Action |
|---------|---------|--------|
| Growing/Stable | High | **Keep** — monitor, update annually |
| Growing/Stable | Low | **Refresh** — rewrite with current information and voice |
| Declining | High | **Optimize** — update keywords, add internal links, refresh intro |
| Declining | Low | **Consolidate or Remove** — merge into a stronger article or 301 redirect |
| Dead | Any | **Evaluate** — is the topic worth covering? If yes, rewrite from scratch. If no, remove and redirect. |

5. **Gap analysis.** Compare your content inventory against your pillars and buyer stages. Where are the holes? These gaps feed directly into your content calendar.

### Audit Frequency

- **Full audit:** twice per year
- **Quick spot-check:** monthly (review the 10 highest-traffic and 10 lowest-traffic pages)
- **Post-redesign audit:** immediately after any site restructure, migration, or rebrand

## Content Distribution Checklist

Publishing is not distribution. A blog post that goes live with no promotion relies entirely on SEO, which takes months. Use this checklist for every piece of content:

**Day of publish:**
- Share on all owned social channels (platform-specific formatting, not identical posts)
- Send to email list (if the content warrants it; not every blog post needs an email)
- Post in relevant communities (Slack groups, Discord servers, Reddit, forums) with genuine context, not just a link drop
- Share internally so sales and support teams can reference it

**Week after publish:**
- Repurpose into 3-5 social posts across platforms (see `social-content` module)
- Add internal links from 2-4 existing related articles
- Submit to relevant newsletters or aggregators in your industry

**Month after publish:**
- Review traffic and engagement data
- Identify underperforming content and diagnose: wrong keyword target, weak title, missing internal links, or just too early to judge?
- Update the meta description if click-through rate is below 3%

## Content Measurement

### Key Metrics by Content Type

| Content Type | Primary Metric | Secondary Metrics |
|-------------|---------------|-------------------|
| Blog post (searchable) | Organic traffic | Keyword rankings, time on page, bounce rate |
| Blog post (shareable) | Social shares and backlinks | Referral traffic, email signups |
| Landing page | Conversion rate | Bounce rate, scroll depth, CTA clicks |
| Case study | Influence on pipeline | Page views from sales-shared links, time on page |
| Email | Click-through rate | Open rate, unsubscribe rate, conversion |
| Social post | Engagement rate | Profile visits, link clicks, follower growth |

### Content ROI Framework

For each piece of content, track:

1. **Input cost:** time to produce (research + writing + editing + design) in hours, multiplied by hourly rate
2. **Output value:** traffic generated, leads captured, deals influenced (attribute using UTM parameters and CRM data)
3. **Content ROI:** (Output value - Input cost) / Input cost

Not every piece needs a positive ROI immediately. Pillar pages and foundational content may take 6-12 months to compound. But if a content type consistently shows negative ROI after 6 months, stop producing it and redirect resources.

## Content Calendar

### Weekly Planning Template

| Day | Activity |
|-----|----------|
| Monday | Review metrics from last week. Identify what performed and what flopped. Adjust plan if needed. |
| Tuesday-Wednesday | Draft new content (batch writing sessions, 2-3 hours minimum) |
| Thursday | Edit, optimize, add visuals, internal links |
| Friday | Publish and distribute (social, email, communities) |

### Monthly Planning

At the start of each month:

1. Review the previous month: which content drove traffic, leads, or engagement?
2. Check the prioritized backlog and pull the top items
3. Assign 4-6 pieces to the month (one major piece per week, adjusted for team capacity)
4. Map each piece to its pillar, buyer stage, and target keyword
5. Schedule publish dates and distribution plan

### Batching Strategy

Batch similar work together. Context-switching between writing, editing, and promotion kills productivity.

- **Research days:** keyword research, competitor analysis, ideation
- **Writing days:** drafting 2-3 pieces in focused blocks
- **Editing days:** review, polish, optimize, add links and visuals
- **Distribution days:** publish, schedule social posts, send newsletters, share in communities

### Cadence by Content Type

| Content Type | Recommended Cadence | Notes |
|-------------|---------------------|-------|
| Blog post | 1-2 per week | Consistency matters more than volume |
| Case study | 1-2 per month | Limited by customer availability |
| Landing page | As needed | Driven by campaigns and product launches |
| Email newsletter | Weekly or biweekly | Pick one cadence and stick to it |
| Social posts | 3-5 per week per platform | Repurpose blog content to maintain volume |
| Documentation | Continuous | Update with product changes |
| White paper | Quarterly | High-effort, high-impact gated content |

## Common Tasks

1. **Define content pillars** — analyze product, audience, search data, and competitive landscape. Propose 3-5 pillars with validation against the four criteria (Relevance, Depth, Expertise, Demand).

2. **Map topics to buyer stages** — take a list of content ideas and categorize each by buyer stage (Awareness, Consideration, Decision, Implementation). Identify gaps in coverage.

3. **Prioritize content ideas** — score a backlog of ideas using the four-factor framework. Return a ranked list with scores and reasoning.

4. **Plan a content calendar** — given pillars, priorities, and team capacity, produce a 4-week editorial calendar with topics, target keywords, content types, and publish dates.

5. **Audit existing content** — review published content against pillars and buyer stages. Identify: content that needs updating, content that should be consolidated, gaps that need filling, and content that should be retired.

6. **Build a topic cluster** — given a pillar, research and map out the full cluster: pillar page, 8-15 subtopics, target keywords, internal linking plan, and publishing sequence.

7. **Conduct keyword research** — for a given pillar or topic area, produce a keyword map organized by buyer stage with volume estimates and difficulty ratings.

## Tips

- Start with fewer pillars (3) and expand to 5 only after you've built depth in the first three. Breadth without depth signals low authority to search engines and readers.
- The best content ideas come from real conversations, not keyword tools. Use keyword tools to validate and refine, not to originate.
- Publish consistently. One post per week for six months beats three posts per week for two months then silence. Search engines and audiences reward predictability.
- Update existing content before creating new content on the same topic. A refreshed article with new data and internal links outperforms a duplicate.
- Every piece of content should have exactly one primary goal: rank for a keyword, generate shares, capture emails, or convert to signup. Content with multiple goals usually achieves none.
- Track content performance at the piece level, not just aggregate. Know which individual articles drive traffic, leads, and revenue.
- Content strategy is not content production. Resist the urge to start writing before the strategy is solid. A week of planning saves months of wasted production.

## Gotchas

- Keyword difficulty scores from tools (Ahrefs, SEMrush) are estimates, not facts. A "low difficulty" keyword can be impossible to rank for if the top results are from high-authority sites. Always review the actual SERP.
- Topic clusters only work if the internal linking is maintained. Every new subtopic article must be linked from the pillar page. Audit links quarterly.
- Content calendars are plans, not promises. If a higher-priority opportunity appears (industry news, product launch, competitive move), adjust the calendar. Rigidity is the enemy of relevance.
- "We should write about [topic]" is not a content idea. A content idea includes: target keyword or angle, buyer stage, content type, and why it matters now. Vague topics die in the backlog.
- Search intent changes over time. A keyword that returned product comparisons last year might now return how-to guides. Re-check intent before writing.
- Competitor content audits can lead to copycat strategies. Use them to find gaps and angles, not to replicate what already exists.

## Related Modules

- `writing/copywriting` — executes the strategy. Takes topics and produces draft copy with headline formulas, CTA patterns, and voice handling.
- `writing/social-content` — distributes and repurposes. Turns blog posts and long-form content into platform-specific social posts.
- `writing/writing-quality` — quality gate. Ensures produced content passes the anti-AI-slop audit before publishing.
