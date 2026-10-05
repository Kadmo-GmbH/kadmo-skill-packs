# Banned AI Writing Patterns

29 patterns sourced from [humanizer](https://github.com/blader/humanizer), based on Wikipedia's "Signs of AI writing" (WikiProject AI Cleanup). Each pattern includes a detection signal, before/after example, and fix guidance.

## Content Patterns (1-6)

### Pattern 1: Significance Inflation
**Signal:** Uses words like "pivotal", "crucial", "vital", "key role", "indelible mark", "setting the stage for"
**Before:** "This pivotal moment in the company's journey underscores the vital role of innovation."
**After:** "The company shipped its first product that quarter."
**Fix:** Replace inflated significance claims with plain facts. If something is important, the facts will show it.

### Pattern 2: Notability Name-Dropping
**Signal:** Mentions famous people, institutions, or events to add unearned weight
**Before:** "Much like Steve Jobs revolutionized personal computing, this approach transforms content creation."
**After:** "This approach reduces content production time by half."
**Fix:** Cut comparisons to famous figures. Make claims stand on their own evidence.

### Pattern 3: Superficial -ing Analyses
**Signal:** Sentences structured as "[Topic], [verb]-ing [vague implication]"
**Before:** "The platform integrates AI agents, enabling teams to ship faster while reducing overhead."
**After:** "The platform uses AI agents. Teams ship twice as fast at a third of the cost."
**Fix:** Split compound sentences. Replace "-ing" clauses with specific, measurable claims.

### Pattern 4: Promotional Language
**Signal:** Words like "boasts", "showcases", "exemplifies", "commitment to", "groundbreaking", "renowned", "breathtaking", "must-visit", "stunning"
**Before:** "The platform boasts a groundbreaking approach to workflow automation."
**After:** "The platform automates 12 workflow types without custom code."
**Fix:** Replace promotional adjectives with specific facts. What does it actually do?

### Pattern 5: Vague Attributions
**Signal:** "Experts say", "studies show", "research indicates", "many believe" without citing sources
**Before:** "Research indicates that AI-augmented teams are significantly more productive."
**After:** "GitHub's 2024 study found developers using Copilot completed tasks 55% faster."
**Fix:** Name the source, cite the study, provide the number. If you can't cite it, cut it.

### Pattern 6: Formulaic Conclusions
**Signal:** "Challenges and future prospects", "looking ahead", "as we move forward", "the future of X is bright"
**Before:** "As we look ahead, the future of AI-powered development is bright, with challenges remaining but prospects encouraging."
**After:** Cut entirely. Or: "Next quarter, the team plans to add Python and Go support."
**Fix:** Delete formulaic conclusion paragraphs. End with a specific next step or CTA, not a vague forward-looking statement.

## Language Patterns (7-13)

### Pattern 7: AI Vocabulary Overuse
**Signal:** Frequent use of: delve, enhance, foster, garner, intricate, landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore, vibrant, crucial, additionally, align with, interplay, valuable, enduring, highlight (as verb)
**Before:** "Let's delve into how this intricate interplay of features enhances the vibrant landscape of modern development."
**After:** "Here's how the features work together."
**Fix:** Replace with plain words. "Delve" → examine, look at. "Enhance" → improve. "Landscape" → field, area. "Leverage" → use.

### Pattern 8: Copula Avoidance
**Signal:** Using "serves as", "stands as", "represents", "marks", "features", "offers" instead of "is"
**Before:** "The dashboard serves as the central hub for fleet monitoring."
**After:** "The dashboard is the fleet monitoring center."
**Fix:** Use "is" or "are" when something simply is something. Fancy verbs aren't more professional.

### Pattern 9: Tailing Negations / Negative Parallelisms
**Signal:** "Not just X, but Y", "not merely X — Y", "X, yet not Y"
**Before:** "This isn't just a tool — it's a complete engineering department."
**After:** "This is a complete engineering department."
**Fix:** State the positive claim directly. Drop the negation setup.

### Pattern 10: Rule of Three Overuse
**Signal:** Lists of exactly three items, especially adjectives or phrases
**Before:** "The platform is fast, reliable, and scalable."
**After:** "The platform handles 10,000 concurrent builds without queueing."
**Fix:** Use two items or one. Three is the strongest AI tell in list construction. If you must list, use specific items, not abstract adjectives.
**Landing page exception:** Reduce to LOW. Feature grids commonly use 3-column layouts, making lists of three a design constraint. Only flag if the three items are vague adjectives rather than specific capabilities.

### Pattern 11: Synonym Cycling
**Signal:** Using different words for the same concept within a paragraph to avoid repetition
**Before:** "The agents complete tasks autonomously. These automated workers handle operations independently. The self-directed bots..."
**After:** "The agents complete tasks autonomously. They handle deployments, tests, and code reviews without human intervention."
**Fix:** Repeat the same term. Readers prefer clarity over forced variety.

### Pattern 12: False Ranges
**Signal:** "From X to Y" constructions that cover everything, saying nothing
**Before:** "From startups to enterprises, from development to deployment, the platform serves diverse needs."
**After:** "The platform runs in production at three companies with 20-500 employees each."
**Fix:** Replace ranges with specific examples or data.

### Pattern 13: Passive Voice / Subjectless Fragments
**Signal:** "It was decided", "improvements were made", "the system can be configured"
**Before:** "Significant improvements were made to the deployment pipeline."
**After:** "The team cut deploy time from 45 minutes to 8."
**Fix:** Name the actor. Who did it? Use active voice.

## Style Patterns (14-19, 26-29)

### Pattern 14: Em Dash Overuse
**Signal:** Multiple em dashes (—) per paragraph
**Before:** "The platform — which launched last quarter — handles everything from deployment — including rollbacks — to monitoring."
**After:** "The platform launched last quarter. It handles deployment (including rollbacks) and monitoring."
**Fix:** Replace em dashes with periods, commas, or parentheses. One em dash per page maximum, if any.

### Pattern 15: Boldface Overuse
**Signal:** Key phrases bolded for emphasis throughout body copy
**Before:** "Our **AI agents** deliver **real results** with **zero configuration**."
**After:** "AI agents deliver real results with zero configuration."
**Fix:** Remove most bold. Use it only for headings, labels, or genuinely critical terms. Body copy should not need bold to communicate emphasis.

### Pattern 16: Inline-Header Vertical Lists
**Signal:** Paragraphs formatted as bold-header + colon + description, stacked vertically
**Before:** "**Speed:** Deploy in minutes. **Scale:** Handle any workload. **Reliability:** 99.9% uptime."
**After:** "Deploy in minutes. It handles any workload at 99.9% uptime."
**Fix:** Rewrite as flowing prose. Lists are fine for genuinely discrete items, but marketing benefits should flow as narrative.

### Pattern 17: Title Case Headings
**Signal:** Every Word In The Heading Is Capitalized
**Before:** "How Our Platform Transforms Your Engineering Workflow"
**After:** "How our platform transforms your engineering workflow"
**Fix:** Use sentence case. Title case is an AI tell in body content and subheadings.

### Pattern 18: Emoji Usage
**Signal:** Emojis used as section markers, bullet decorations, or emphasis
**Fix:** Remove emojis from professional content. Exception: social media posts where emojis are platform-native.

### Pattern 19: Curly Quotation Marks
**Signal:** "Smart quotes" instead of "straight quotes" in technical or web content
**Fix:** Use straight quotes for web content. Curly quotes can cause encoding issues and are an AI formatting tell.

### Pattern 26: Hyphenated Word Pair Overuse
**Signal:** Frequent compound modifiers: "cutting-edge", "data-driven", "results-oriented", "mission-critical"
**Before:** "Our cutting-edge, AI-powered, data-driven platform delivers mission-critical results."
**After:** "The platform uses AI to analyze data and produce reliable results."
**Fix:** Unpack hyphenated pairs into plain descriptions. One per sentence maximum.

### Pattern 27: Authority Tropes
**Signal:** "The real question is", "at its core", "in reality", "what really matters", "fundamentally", "the heart of the matter"
**Before:** "At its core, the real question is what fundamentally matters for your engineering team."
**After:** "Your engineering team needs faster deploys and fewer bugs."
**Fix:** Delete the authority framing. State the point directly.

### Pattern 28: Signposting Announcements
**Signal:** "Let's dive in", "let's explore", "let's break this down", "here's what you need to know", "without further ado"
**Before:** "Let's dive into how AI agents can transform your workflow."
**After:** "AI agents pick tickets, write code, and open PRs. Here's how."
**Fix:** Delete the signpost. Start with the content.

### Pattern 29: Fragmented Headers
**Signal:** Headers that are sentence fragments used for dramatic effect
**Before:** "The problem. → The solution. → The results."
**After:** "Problem → Solution → Results" or just use descriptive headers
**Fix:** Use descriptive headers or full sentences. Dramatic fragment headers are a strong AI tell.
**Landing page exception:** Suppress. Landing pages use fragment headers as section labels paired with supporting body text (e.g., "Deploy." + "Set up agents on dedicated cloud servers."). This is standard web design, not an AI pattern.

## Communication Patterns (20-22)

### Pattern 20: Chatbot Artifacts
**Signal:** "I hope this helps!", "Of course!", "Certainly!", "You're absolutely right!", "Would you like me to...", "Let me know if..."
**Fix:** Delete entirely. Published content should never contain conversational artifacts.

### Pattern 21: Knowledge-Cutoff Disclaimers
**Signal:** "As of my last update", "I don't have access to real-time data", "based on information available to me"
**Fix:** Delete entirely. If information might be outdated, verify it or note the date.

### Pattern 22: Sycophantic Tone
**Signal:** Excessive agreement, complimenting the reader, "great question!", "that's a really interesting point"
**Fix:** Delete. State information directly without complimenting the audience.

## Filler & Hedging (23-25)

### Pattern 23: Filler Phrases
**Signal:** "It's worth noting that", "it's important to remember that", "it should be mentioned that", "needless to say"
**Before:** "It's worth noting that the platform handles concurrent deployments efficiently."
**After:** "The platform handles concurrent deployments efficiently."
**Fix:** Delete the filler phrase. The sentence is always stronger without it.

### Pattern 24: Excessive Hedging
**Signal:** "Arguably", "perhaps", "it could be said that", "in some ways", "to some extent", "relatively"
**Before:** "The platform is arguably one of the more effective solutions in the space."
**After:** "The platform ships code faster than manual engineering teams."
**Fix:** Commit to the claim or don't make it. Hedging signals uncertainty without adding nuance.

### Pattern 25: Generic Positive Conclusions
**Signal:** Final paragraphs that summarize what was said and end on a vague positive note
**Before:** "In conclusion, AI-powered development represents an exciting frontier with tremendous potential to transform how teams build software."
**After:** "Start a pilot with three agents on your lowest-risk codebase. Results show in the first sprint."
**Fix:** End with a specific action, not a summary. If the content was good, it doesn't need restating.

## Structural Patterns (S1-S6)

These patterns come from [stop-slop](https://github.com/hardikpandya/stop-slop) and address higher-level structural tells.

### Pattern S1: Binary Contrasts
**Signal:** "Not because X. Because Y.", "X isn't the problem. Y is.", "The answer isn't X. It's Y."
**Before:** "The problem isn't the technology. It's the process."
**After:** "The process needs fixing. Here's how."
**Fix:** State the positive claim directly. Drop the false contrast.

### Pattern S2: Negative Listing
**Signal:** "Not a X... Not a Y... A Z." (Rhetorical striptease through negation)
**Before:** "Not a dashboard. Not a report. A complete operating system for your engineering team."
**After:** "A complete operating system for your engineering team."
**Fix:** Skip the buildup. State what it is.

### Pattern S3: Dramatic Fragmentation
**Signal:** "[Noun]. That's it. That's the [thing].", "[X]. And [Y]. And [Z]."
**Before:** "Ship. Review. Deploy. That's it. That's the workflow."
**After:** "The workflow has three steps: ship, review, deploy."
**Fix:** Write complete sentences. Fragmentation for drama is one of the most recognizable AI patterns.

### Pattern S4: False Agency
**Signal:** Inanimate objects performing human actions: "a complaint becomes a fix", "the data tells us", "the market rewards", "the decision emerges"
**Before:** "The platform empowers teams to achieve their goals."
**After:** "Teams using the platform ship 2x faster."
**Fix:** Name human actors. "The market rewards" → "Customers buy". "The data tells us" → "We found".

### Pattern S5: Narrator-from-Distance
**Signal:** "Nobody designed this.", "This happens because...", "People tend to...", "It turns out..."
**Before:** "Nobody planned for this outcome. It emerged naturally from the architecture."
**After:** "The architecture produced this outcome. We didn't plan for it."
**Fix:** Name actors and use first person where appropriate. Avoid the omniscient narrator voice.

### Pattern S6: Rhetorical Setups
**Signal:** "What if [reframe]?", "Here's what I mean:", "Think about it:", "And that's okay."
**Before:** "What if I told you that AI agents could handle 90% of your engineering tasks? Think about it."
**After:** "AI agents handle 90% of engineering tasks in our production fleet."
**Fix:** Delete the setup. State the claim.
