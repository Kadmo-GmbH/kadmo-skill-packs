---
name: Content Quality
domain: writing
parent: writing
tags: ["@quality", "@anti-slop", "@humanizer"]
confidence: 0.49
upstream:
  - blader/humanizer (MIT)
  - hardikpandya/stop-slop (MIT)
---

# Writing Quality

A writing quality gate that detects and eliminates AI-generated writing patterns. Merges 29 detection patterns from [humanizer](https://github.com/blader/humanizer) with 5-dimension scoring from [stop-slop](https://github.com/hardikpandya/stop-slop). Use this module as the final quality check before publishing any content.

See `ATTRIBUTION.md` for licensing details.

## Two-Pass Audit Process

### Pass 1: Pattern Detection

Scan the content for AI writing patterns across 6 categories. Each detected pattern is a signal that the text sounds machine-generated.

| Category | Patterns | What to look for |
|----------|----------|-----------------|
| **Content** | 1-6 | Significance inflation, notability name-dropping, superficial analyses, promotional language, vague attributions, formulaic conclusions |
| **Language** | 7-13 | AI vocabulary overuse, copula avoidance ("serves as" instead of "is"), tailing negations, rule of three, synonym cycling, false ranges, passive voice |
| **Style** | 14-19, 26-29 | Em dash overuse, boldface overuse, inline-header lists, title case, emojis, hyphenated pairs, authority tropes, signposting, fragmented headers |
| **Communication** | 20-22 | Chatbot artifacts ("I hope this helps!"), knowledge-cutoff disclaimers, sycophantic tone |
| **Filler & Hedging** | 23-25 | Filler phrases, excessive hedging, generic positive conclusions |
| **Structural** | S1-S6 | Binary contrasts, negative listings, dramatic fragmentation, rhetorical setups, false agency, narrator-from-distance |

Full pattern catalog with before/after examples: `references/banned-patterns.md`

### Pass 2: Scoring & Verdict

Rate the content 1-10 on five dimensions. The scoring system measures how human the writing sounds, not whether it's "good" writing in general.

| Dimension | Measures | 7+ (good) | 3- (bad) |
|-----------|----------|-----------|----------|
| **Directness** | Statements vs announcements | Direct assertions, confident claims | Throat-clearing, "here's why this matters" |
| **Rhythm** | Sentence length variation | Natural mix of short and long | Every sentence roughly the same length |
| **Trust** | Reader respect | States facts, lets reader conclude | Over-explains, hedges, hand-holds |
| **Authenticity** | Human voice | Specific, opinionated, some rough edges | Generic, corporate, sanitized |
| **Density** | Signal-to-noise | Every word earns its place | Filler phrases, redundant qualifiers |

**Threshold: 35/50 to pass.** Any single dimension below 5 = automatic revision, regardless of total score.

Full rubric with scoring examples: `references/scoring.md`

## Core Rules

These 8 rules from stop-slop are the active editing directives. Apply them when rewriting flagged content:

1. **Cut filler phrases** — if removing a phrase doesn't change the meaning, remove it. See `references/banned-phrases.md`.
2. **Break formulaic structures** — binary contrasts, dramatic fragments, rhetorical striptease. See `references/banned-structures.md`.
3. **Use active voice** — every sentence needs a named human subject doing something. "The decision was made" → "Sarah decided."
4. **Be specific** — replace vague declaratives with concrete details. "The results were significant" → "Output increased 2.4x."
5. **Put the reader in the room** — "you" beats "people", "users", "one." Second person creates immediacy.
6. **Vary rhythm** — mix sentence lengths. Two items in a list beat three. No em dashes (use commas or periods instead).
7. **Trust readers** — state facts directly. Skip softening ("it's worth noting that"), hedging ("arguably"), and meta-commentary ("let me explain").
8. **Cut quotables** — if a sentence sounds like a pull-quote or inspirational poster, rewrite it. Real writing doesn't pose.

## Quick Checks

A 12-item pre-delivery checklist. Run mentally before submitting any content:

1. Any adverbs (-ly words)? Cut them or replace with stronger verbs.
2. Any passive voice? Name the actor.
3. Any false agency? (Objects doing human things: "the data tells us", "the market rewards")
4. Any paragraphs starting with Wh- words? (What, When, Where, Which, Who, Why, How) Restructure.
5. Any throat-clearing openers? ("Here's the thing:", "Let me be clear:") Delete.
6. Any binary contrasts? ("Not X. But Y.") Rewrite as a direct statement.
7. All sentences roughly the same length? Vary them.
8. Every paragraph ending punchily? That's a pattern — vary endings too.
9. Any em dashes? Replace with commas, periods, or parentheses.
10. Any vague declaratives? ("The implications are significant.") Replace with specifics.
11. Any narrator-from-distance? ("Nobody designed this.", "This happens because...") Name actors.
12. Any meta-joiners? ("And that's okay.", "Here's what I mean:", "Think about it:") Cut.

## Voice Calibration

When rewriting content to remove AI patterns, don't strip all personality. Good human writing has:

- **Opinions** — take a stance, don't hedge everything
- **Varied rhythm** — mix short punchy sentences with longer flowing ones
- **Specificity** — concrete details, not abstract claims
- **First person** (when appropriate) — "I" and "we" are fine in blog posts and social
- **Imperfection** — not every transition needs to be smooth. Some mess is human.
- **Tension** — good writing creates and resolves small tensions. Don't flatten everything.

If a brand context is available, calibrate voice to match its tone and personality. The goal is to sound like a specific human writer, not a generic one.

## Common Tasks

1. **Full quality check** — run both passes (pattern detection + scoring), return findings and verdict
2. **Quick scan** — pattern detection only, no scoring. For fast feedback during drafting.
3. **Targeted revision** — fix specific pattern categories (e.g., "fix only filler and structural patterns")
4. **Score only** — skip pattern detection, just rate on 5 dimensions. For content that's already been through pattern cleanup.
5. **Voice calibration** — analyze a writing sample and extract style fingerprint. Use when adapting content to a specific brand voice.

## Tips

- Don't just remove bad patterns — inject personality. Copy that's merely "not AI-sounding" is still bland.
- Two items in a list always beat three. The rule of three is one of the strongest AI tells.
- Read the content aloud (mentally). If it sounds like a keynote speech, it's too polished.
- After cleaning up AI patterns, re-read the whole piece. Sometimes cleanup strips voice. Add it back.
- Some patterns are occasionally correct in context. An em dash can be fine in informal writing. A list of three can work if the items are genuinely distinct. Use judgment.

## Gotchas

- Voice calibration can over-correct, making copy bland and lifeless. Always re-read after cleanup.
- The blanket adverb ban (rule 1) is aggressive. "Quickly" is usually cuttable, but "previously" is sometimes necessary. Apply with judgment.
- False agency (S4) is one of the strongest AI tells but also one of the hardest to fix without restructuring sentences significantly.
- Narrator-from-distance (S5) detection can false-positive on legitimate explanatory writing. Context matters.
- The 35/50 threshold is calibrated for marketing copy. Technical documentation may legitimately score lower on Rhythm and Authenticity dimensions without being "AI-sounding."

## References

- `references/banned-patterns.md` — full 29-pattern catalog with detection signals, before/after examples, and fix guidance
- `references/banned-phrases.md` — categorized phrase lists: throat-clearing, emphasis crutches, jargon, adverbs, meta-commentary
- `references/banned-structures.md` — structural anti-patterns: binary contrasts, false agency, dramatic fragmentation
- `references/scoring.md` — 5-dimension rubric with detailed 1-10 descriptors and scoring examples
