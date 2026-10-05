# 5-Dimension Scoring Rubric

Score content 1-10 on each dimension. The scoring system measures how human the writing sounds, not general writing quality.

**Threshold: 35/50 to pass. Any single dimension below 5 = automatic revision.**

## Dimension 1: Directness

Does the content make statements or announce them?

| Score | Description |
|-------|-------------|
| 1-2 | Every paragraph opens with a throat-clearing phrase. Claims are wrapped in disclaimers. |
| 3-4 | Frequent "here's the thing" or "it's worth noting" constructions. Gets to the point eventually. |
| 5-6 | Generally direct with occasional throat-clearing. Most sentences lead with the claim. |
| 7-8 | Confident, direct assertions. No unnecessary framing. States facts and moves on. |
| 9-10 | Every sentence leads with its point. Zero filler. Reads like someone who respects the reader's time. |

**Detection:** Count throat-clearing openers, emphasis crutches, and meta-commentary phrases. More than 2 per 500 words = score below 5.

## Dimension 2: Rhythm

Are sentence lengths varied or metronomic?

| Score | Description |
|-------|-------------|
| 1-2 | Every sentence is 12-18 words. Reading it feels like a metronome. |
| 3-4 | Mostly uniform length with occasional variation. Lists always have three items. |
| 5-6 | Some variation. A few short sentences mixed with medium ones. Paragraphs roughly uniform length. |
| 7-8 | Natural variation. Short punches (4-6 words) mixed with longer explanations (20-25 words). Paragraphs vary too. |
| 9-10 | Reads like natural speech. Sentence length matches content gravity — short for impact, long for nuance. No patterns. |

**Detection:** Measure sentence word counts in a paragraph. Standard deviation below 3 = score below 5. Any rule-of-three lists = subtract 1 point per occurrence.

## Dimension 3: Trust

Does the content respect the reader's intelligence?

| Score | Description |
|-------|-------------|
| 1-2 | Over-explains everything. Hedges every claim. Treats the reader as a beginner even when the topic is advanced. |
| 3-4 | Frequent hedging ("arguably", "perhaps"). Explains obvious implications. Uses "as you know" or "as we all know." |
| 5-6 | Mostly trusts the reader. Occasional unnecessary explanations or softening. |
| 7-8 | States facts and lets the reader draw conclusions. No hand-holding. Confident without being arrogant. |
| 9-10 | Assumes competence. Presents evidence and trusts the reader to evaluate it. Never condescends. |

**Detection:** Count hedging words, "as you know" constructions, and instances where an implication is explicitly stated after the evidence already implies it.

## Dimension 4: Authenticity

Does the content sound like a specific human wrote it?

| Score | Description |
|-------|-------------|
| 1-2 | Generic corporate tone. Could be any company, any product, any writer. No personality. |
| 3-4 | Mostly generic with occasional flashes of voice. Uses AI vocabulary (delve, landscape, leverage). |
| 5-6 | Has a recognizable tone but doesn't take risks. Safe, competent, forgettable. |
| 7-8 | Sounds like a person. Has opinions, specific details, and occasional imperfection. Uses first person naturally. |
| 9-10 | Unmistakably human. Takes stances, acknowledges uncertainty where real, includes specific anecdotes or observations. Could not have been written by anyone else. |

**Detection:** Check for AI vocabulary (Pattern 7), synonym cycling (Pattern 11), and generic positive conclusions (Pattern 25). More than 3 AI vocabulary words per 500 words = score below 5.

## Dimension 5: Density

Is there anything cuttable?

| Score | Description |
|-------|-------------|
| 1-2 | Bloated. Every sentence has 2-3 unnecessary words. Paragraphs could be halved without losing meaning. |
| 3-4 | Noticeable padding. Filler phrases ("it's worth noting", "needless to say") throughout. Redundant qualifiers. |
| 5-6 | Mostly tight with occasional loose sentences. A careful editor would cut 10-15%. |
| 7-8 | Lean. Every word contributes. A careful editor might cut 5%. |
| 9-10 | Nothing to cut. Every word earns its place. Removing any sentence would leave a gap. |

**Detection:** Count filler phrases (Pattern 23), adverbs, and redundant qualifiers. Try removing sentences — if meaning is preserved, density is low.

## Scoring Process

1. Read the full content once without scoring
2. Rate each dimension independently (don't let one score influence others)
3. Sum the five scores
4. Check for any dimension below 5 (automatic revision trigger)
5. Apply the verdict:

| Total | Verdict | Action |
|-------|---------|--------|
| 40-50 | **Strong pass** | Publish-ready. Minor polish only. |
| 35-39 | **Pass** | Acceptable. Address any specific findings. |
| 30-34 | **Borderline** | Revise targeted areas. Re-score after revision. |
| 25-29 | **Revise** | Significant revision needed. Multiple dimensions weak. |
| Below 25 | **Rewrite** | Fundamental issues. Consider starting fresh with different approach. |

## Scoring Example

**Sample text:** "It's worth noting that the platform serves as a comprehensive solution for modern engineering teams. By leveraging cutting-edge AI technology, it enables organizations to streamline their development workflows. The implications are significant."

**Scores:**
- Directness: 3 — opens with "it's worth noting", buries the claim
- Rhythm: 2 — all three sentences are 12-16 words, metronomic
- Trust: 3 — "it's worth noting" and "implications are significant" explain nothing
- Authenticity: 2 — "serves as", "leveraging", "cutting-edge", "streamline" = AI vocabulary
- Density: 2 — could be one sentence: "The platform automates engineering workflows with AI."
- **Total: 12/50 — Rewrite**

**Revised:** "The platform automates engineering workflows with AI agents. Teams using it ship code twice as fast."

**Revised scores:** Directness 8, Rhythm 7, Trust 8, Authenticity 7, Density 9 = **39/50 — Pass**

## Content-Type Adjustments

The default rubric is calibrated for **prose** (articles, blog posts, documentation). Other content types have different norms. Apply the adjustments below based on detected content type.

### Landing Pages

Landing pages use intentionally uniform structure: short headlines, short supporting sentences, 3-column feature grids, fragment section labels. These are design patterns, not AI tells.

**Dimension adjustments:**

| Dimension | Default norm | Landing page norm | Why |
|-----------|-------------|-------------------|-----|
| **Rhythm** | Varied sentence lengths (short + long) | Uniform short is intentional | Landing pages pair a 3-5 word headline with a 10-20 word description, repeated per section. This is a layout constraint, not metronomic writing. Score 5+ if the copy matches its visual structure. |
| **Authenticity** | Sounds like one specific person | Brand voice, not personal voice | Landing pages are team-written, brand-voiced. "Sounds like one person" is the wrong test. Score based on whether the copy has a distinct brand personality vs. generic corporate tone. Score 5+ if the voice is consistent and differentiated. |
| **Trust** | Inline evidence for every claim | Specific and verifiable claims | Landing pages source metrics through linked case studies, annual reports, footnotes, or dedicated proof sections — not inline citations. Judge Trust based on whether claims are **specific and verifiable** (named customers, concrete numbers, named sources like "G2", "Forbes"), not whether the source URL appears inline. A claim like "US$1.9tn total volume in 2025" with a named company is verifiable. A claim like "trusted by thousands" is not. Score 5+ if most metrics are specific enough to be verified and attributed to named entities. Only flag as trust issues: vague unverifiable claims ("industry-leading", "thousands of customers"), absolute superlatives without any qualifier ("perfect, every time"), and metrics with no indication of origin or scope. |

**Threshold adjustment:**

| Total | Verdict (landing page) | Action |
|-------|----------------------|--------|
| 35-50 | **Strong pass** | Publish-ready. |
| 28-34 | **Pass** | Acceptable for landing pages. Address specific findings. |
| 22-27 | **Borderline** | Revise targeted areas. |
| Below 22 | **Revise** | Significant issues even for landing page format. |

**Pattern suppression for landing pages:**
- Suppress: Fragmented Headers (Pattern 29), Dramatic Fragmentation (S3)
- Reduce to LOW: Rule of Three (Pattern 10), Metronomic Sentences
- Keep full severity: All other patterns (promotional language, vague claims, AI vocabulary, etc.)

### Social Posts

Short-form content. Fragments and hooks are native to the format.

- Suppress: Fragmented Headers, Dramatic Fragmentation, Em Dash Overuse
- Reduce to LOW: Rule of Three, Metronomic Sentences
- Threshold: 22/50 to pass

### Email

Similar to prose but shorter paragraphs are expected.

- Keep all patterns at full severity
- Threshold: 30/50 to pass
