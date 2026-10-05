---
name: Storybook Testing Workflow
domain: storybook
path: /
tags: ["@engineering", "@dev", "@frontend", "@react", "@testing"]
learned: "2026-08-08"
last_verified: "2026-08-12"
confidence: 0.75
---

# Storybook Testing Workflow

A story is the cheapest reviewable proof that a component change does what the ticket says — and
since Storybook 9 it can also be a real CI gate. This pack covers writing stories that function as
review artefacts and wiring them into test gates, on the current major, with the package moves that
silently break v8-era configuration called out explicitly.

Package facts were verified against `registry.npmjs.org` and the shipped `@storybook/react` 10.5.7
types on 2026-08-08. Storybook majors move quickly and renames land without runtime errors —
re-check the version selector on any docs page before copying from it.

## Module Inventory

| Module | Covers | `_skill.md` |
|--------|--------|:-----------:|
| [testing-workflow](testing-workflow/_skill.md) | Current-major `main.ts`/addon setup, theme decorators, CSF 3 stories, `play` interaction tests, the Vitest addon gate, the a11y ratchet, snapshot vs visual coverage | 0.75 |

## Authoring Rules

1. **Name the major for every idiom.** Most Storybook advice is major-specific and fails silently
   on the wrong one — a removed config key just stops working.
2. **Shipped types over docs.** Where docs and the published package disagree (exports, type
   parameters), the shipped `.d.ts` wins and the claim cites it.
3. **No application-specific claims.** Which major and builder a given product runs belongs in
   that consumer's own domain pack, never here.
