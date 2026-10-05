# Banned Structures

Structural patterns that signal AI-generated writing. These are higher-level tells — they affect paragraph and section organization, not just word choice. Sourced from [stop-slop](https://github.com/hardikpandya/stop-slop).

## Binary Contrasts

The "not this, but that" construction. AI uses it to create false tension.

**Patterns to detect:**
- "Not because X. Because Y."
- "[X] isn't the problem. [Y] is."
- "The answer isn't X. It's Y."
- "stops being X and starts being Y"
- "less about X, more about Y"
- "X is dead. Long live Y."
- "The question isn't X. It's Y."
- "Forget X. Think Y."
- "It's not X vs Y. It's X and Y."
- "The shift from X to Y"
- "Move beyond X. Embrace Y."

**Fix:** State the positive claim directly. "The answer isn't speed. It's reliability." → "Reliability matters more than speed here."

## Negative Listing

Rhetorical striptease through negation. Builds up by saying what something isn't.

**Pattern:** "Not a X... Not a Y... A Z."

**Example:** "Not a framework. Not a library. Not even a platform. A complete engineering department."

**Fix:** "A complete engineering department." Skip the buildup.

## Dramatic Fragmentation

One-word or one-phrase sentences stacked for rhythm.

**Patterns:**
- "[Noun]. That's it. That's the [thing]."
- "X. And Y. And Z."
- "Simple. Fast. Reliable."
- "One team. One sprint. Ten PRs."
- "[Verb]. [Verb]. [Verb]."

**Fix:** Write complete sentences. "Ship. Review. Deploy." → "The workflow moves from shipping to review to deployment."

**Landing page exception:** Suppress. Landing pages use fragment headlines as section labels paired with supporting descriptions. "Deploy. Set up agents on dedicated servers." is a headline + body pair, not dramatic fragmentation. Only flag fragments that stack 3+ in a row with no supporting text.

## Rhetorical Setups

Questions or frames designed to create a dramatic pause before the real point.

**Patterns:**
- "What if [reframe]?"
- "Here's what I mean:"
- "Think about it:"
- "And that's okay."
- "Let me explain."
- "Consider this:"
- "Imagine [scenario]."

**Fix:** Delete the setup. Start with the point. "What if you could deploy 10x faster? Here's how." → "Deploy 10x faster with parallel agent execution."

## False Agency

Inanimate objects performing human actions. AI does this to avoid naming actors.

**Patterns:**
- "a complaint becomes a fix"
- "a bet lives or dies"
- "the decision emerges"
- "the data tells us"
- "the market rewards"
- "the platform empowers"
- "the tool enables"
- "the numbers speak for themselves"
- "the code reviews itself"
- "results followed"

**Fix:** Name human actors. "The data tells us deployment speed matters." → "We found that faster deploys reduced rollbacks by 60%."

## Narrator-from-Distance

An omniscient narrator describing events from outside, as if watching from above.

**Patterns:**
- "Nobody designed this."
- "This happens because..."
- "People tend to..."
- "Teams often find themselves..."
- "It's a pattern we see again and again."
- "The industry has shifted toward..."

**Fix:** Use first person or name specific actors. "People tend to underestimate deployment complexity." → "We underestimated deployment complexity. It cost us two sprints."

## Passive Voice

Every sentence needs a named actor. Passive voice is the single most common structural tell.

**Patterns:**
- "The system was designed to..."
- "Improvements were made..."
- "It was decided that..."
- "The feature can be configured..."
- "Tests are run automatically..."

**Fix:** "Improvements were made to the pipeline." → "The team improved the pipeline." Name who did what.

## Rhythm Patterns

### Metronomic Sentences
Every sentence roughly the same length (12-18 words). Vary between 4-word punches and 25-word explanations.

**Landing page exception:** Reduce to LOW. Landing pages intentionally repeat a headline + description structure across sections. Uniform length is a layout pattern, not a rhythm defect.

### Rule of Three
Lists of exactly three items. Use two or four. Three is the strongest AI tell in list construction.

**Landing page exception:** Reduce to LOW. Three-column feature grids are a standard web design pattern. Only flag when three vague adjectives are stacked ("fast, reliable, scalable"), not when three specific features are listed.

### Punchy Paragraph Endings
Every paragraph ending with a short, impactful sentence. Vary your endings.

### Question-Answer Pairs
"How does it work? Simple." — questions immediately answered in the next sentence. Rephrase as direct statements.

## Word Patterns

### Lazy Extremes
- Every, always, never, everyone, nobody, nothing, everything
- Replace with specific quantities or qualifiers.

### All Adverbs
- Any -ly word is suspect. See `banned-phrases.md` for the full list.
- Replace with stronger verbs: "moved quickly" → "sprinted"
