---
name: Wikipedia
domain: wikipedia.org
match:
  - wikipedia.org
  - en.wikipedia.org
tags: [research, reference, knowledge]
---

# Wikipedia

Article extraction, content reading, and summarization. Agents read the public article namespace, the talk pages, and category indexes. No login needed for reads; editing is out of scope for this pack.

## Browser Access

No login required. Wikipedia is fully public and uncached behind authentication. The same selectors work across language editions because all Wikimedia wikis share the MediaWiki theme — only the URL host changes (`en.wikipedia.org`, `de.wikipedia.org`, `fr.wikipedia.org`, etc.).

## Page types

| URL pattern | Purpose |
|---|---|
| `/wiki/<Title>` | Article (default namespace) |
| `/wiki/Talk:<Title>` | Discussion page for the article |
| `/wiki/Category:<Name>` | Category index — lists member pages |
| `/wiki/File:<Name>` | Image / media metadata page |
| `/wiki/Special:<Page>` | Auto-generated tools (Random, RecentChanges, Search) |
| `/w/index.php?title=<Title>&action=history` | Revision history |
| `/w/index.php?title=<Title>&action=edit` | Wiki source view (read-only safe) |

## Article structure

Every article uses the same anatomy:

| Section | Selector | Notes |
|---|---|---|
| Title | `#firstHeading` | Canonical page title, H1 |
| Lead paragraph | First `<p>` inside `#mw-content-text` | Best single-paragraph summary |
| Infobox | `.infobox` | Right-rail key/value table — birth dates, founding years, taxonomy |
| Table of contents | `#toc` | Anchor navigation built from `## ` / `### ` headings |
| Body | `#mw-content-text > .mw-parser-output` | All section bodies |
| References | `.references`, `ol.references li` | Numbered inline citation list |
| See also | `#See_also` heading section | Editor-curated related links |
| External links | `#External_links` heading section | Outbound primary sources |

Infoboxes are the most data-rich part of the page and parse cleanly into key/value pairs. They are the right target when extracting structured facts (population, area, capital, founder, etc.).

## Disambiguation

When a title resolves to a disambiguation page, MediaWiki adds `<div class="hatnote">` at the top and the body becomes a list of links. Detect this with the page categories: disambiguation pages carry `Category:Disambiguation_pages`. When found, fall back to a more specific search query rather than extracting the page body as if it were an article.

## Citations and references

Inline citations render as superscript anchor links (`<sup id="cite_ref-…">[1]</sup>`) that point to entries in `.references`. Each reference list item contains the citation text plus an outbound link to the source. To extract a citation graph for an article, walk the references list and resolve each `<a>` href.

## Categories

The footer of every article shows its categories at `#mw-normal-catlinks`. Categories form a graph: each is itself a `/wiki/Category:<Name>` page that lists members. Walking categories breadth-first is the standard way to enumerate "all articles about X" without using the search index.

## Common tasks

**Summarize article:** navigate to `/wiki/<Title>`, extract the lead paragraph (first `<p>` inside `#mw-content-text`) and the first sentence of each top-level section, hand to an LLM for synthesis.

**Extract content:** navigate to article URL, snapshot `#mw-content-text`, optionally drop `.reference`, `.mw-editsection`, `.thumb`, and `.navbox` to clean prose.

**Extract infobox:** select `.infobox tr`, parse `th` (label) + `td` (value) pairs.

**List a category:** navigate to `/wiki/Category:<Name>`, walk pagination through `next page` links to enumerate all members.

## Gotchas

- Some pages use `#mw-content-text` but the parsed output is wrapped in `.mw-parser-output` — both selectors are needed for a robust grab.
- Section anchors are URL-encoded versions of the heading text with spaces as underscores (`#See_also`).
- The mobile site (`en.m.wikipedia.org`) has a different DOM and collapses sections by default — prefer the desktop host.
- Infoboxes vary by article type (Person, Place, Company, Film, Album, …) — keys are not standardized across types.
- Wikipedia rate-limits aggressive crawling. Between articles add a 1–2 second pause and respect any `Retry-After` headers if scraping at volume.
