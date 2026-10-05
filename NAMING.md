# Knowledge Pack Naming Requirements

Rules for naming packs, roles, modules, and workflows so they render correctly across the SideButton website, search results, and agent UIs.

Titles on the website follow a simple template: `{child-name} · {pack-title}`. Google truncates titles around 580px (~65 chars) and flags anything below ~400px (~45 chars) as "too short". To land in the SEO sweet spot (500–580px) without conditional UI logic, every input must meet the minimums below.

## Length minimums

| Field | Min | Max | Example |
|---|---|---|---|
| Pack title | 18 chars | 35 chars | `LinkedIn Outreach Platform` |
| Role display name | 15 chars | 30 chars | `Sales Development` |
| Module display name | 15 chars | 30 chars | `Campaign Diagnostics` |
| Workflow title | 15 chars | 30 chars | `Draft Message Reply` |

Under 15 is too terse for search snippets. Over 35 risks truncating the child name when combined with the pack title.

## Uniqueness rules

1. **No word appears twice across `{child-name} · {pack-title}`.** Word repetition is a ranking-hostile signal flagged by SEO auditors.
   - Bad: `Content Writer · Content Writing` ("Content" repeats)
   - Good: `Content Writer · Writing Standards`

2. **Stems that share a root are acceptable** (`Writer` / `Writing`) — SEO tools flag exact word duplication, not morphological overlap.

3. **Repeating the platform name in children is OK when it's part of a proper product name** — e.g., `GitHub Actions · GitHub Platform`. But prefer natural descriptors that don't duplicate (`Pull Request Review · GitHub Platform`).

## Pack title guidelines

A pack title must be a noun phrase that names both the platform and its role in the agent stack. Common patterns:

- `{Platform} {Descriptor}` — `LinkedIn Outreach Platform`, `Google Ads Search Platform`
- `{Platform} {Product}` — `Jira Cloud`, `Slack Workspace`
- `{Capability} Standards` — `Writing Standards`, `Testing Standards`
- `{Platform} {Product} {Descriptor}` — `Google Ads Search Campaigns`

Avoid bare platform names (`LinkedIn`, `Google Ads`) — they're too short and omit what the pack actually teaches.

## Role name guidelines

Use full, canonical job titles. Avoid abbreviations or single words:

- Bad: `Sales`, `Analyst`, `PM`
- Good: `Sales Development`, `Ads Performance Analyst`, `Project Manager`

Parallel the level of specificity across roles in the same pack. If one role is `Marketing Analyst`, the sibling should be `Media Buyer`, not just `Buyer`.

## Module name guidelines

Describe a concrete capability area, not a noun alone:

- Bad: `Quality`, `Strategy`, `Reports`
- Good: `Content Quality`, `Campaign Strategy`, `Reports & Dashboards`

## Workflow title guidelines

Use `{verb} {object} [{context}]` form, with enough context to be self-describing without the pack name:

- Bad: `Draft Reply`, `Scan`, `Extract`
- Good: `Draft Message Reply`, `Scan Feed for Mentions`, `Extract Order History`

Keep button labels (the YAML `embed.label` field) short (≤12 chars) since they render inside cramped product UIs. The SEO `title:` field is separate and follows the length minimums above.

## Frontmatter consistency

Every pack has three places where display text appears:

1. **`<root>/index.json`** — registry `title` field
2. **`<pack>/skill-pack.json`** — manifest `title` field
3. **`<pack>/_skill.md`** — YAML frontmatter `name:` and `# H1`

All three must match. The website seeds `skill_packs.title` from `skill-pack.json` (the manifest), but inconsistency between these files is confusing for contributors and future tooling.

## Field reference

| File | Field | Purpose |
|---|---|---|
| `<pack>/skill-pack.json` | `title` | Displayed on website pack cards, page titles, breadcrumbs |
| `<pack>/skill-pack.json` | `name` | URL slug (e.g., `linkedin`, not `linkedin.com`) |
| `<pack>/skill-pack.json` | `domain` | Match pattern for agent runtime (e.g., `linkedin.com`) |
| `<pack>/_skill.md` frontmatter `name` | Agent-facing pack name loaded at runtime |
| `<pack>/_roles/<role>.md` frontmatter `name` | Displayed role name on website + in agent role picker |
| `<pack>/<module>/_skill.md` frontmatter `name` | Displayed module name on website |
| `<pack>/<workflow>.yaml` → `title` | Displayed workflow title on website and in portal |
| `<pack>/<workflow>.yaml` → `embed.label` | Button label in product UI overlay (short) |

## Checklist for new packs

- [ ] Pack title is 18–35 chars and includes a descriptor
- [ ] All role names are 15–30 chars and are full job titles
- [ ] All module names are 15–30 chars
- [ ] All workflow `title:` fields are 15–30 chars (verb + object + context)
- [ ] No word appears twice in any `{child-name} · {pack-title}` combination
- [ ] `index.json`, `skill-pack.json`, and `_skill.md` display names match
- [ ] Pack `name` field (slug) is lowercase, kebab-case, no TLDs (`linkedin`, not `linkedin.com`)
