---
name: MUI Breaking Changes
domain: mui
path: /
tags: ["@engineering", "@dev", "@frontend", "@react", "@typescript"]
learned: "2026-08-08"
last_verified: "2026-08-12"
confidence: 0.85
---

# MUI Breaking Changes

MUI is where model training data most likely betrays a React agent: four majors' worth of renamed
and removed APIs sit exactly where a model's v5-era reflexes point (`Grid item xs`,
`componentsProps`, `makeStyles`, `InputProps`), and the official docs at `mui.com` silently serve
the newest major. This pack maps the breakage so an agent's first pass compiles against the major
the codebase actually runs.

Everything registry-verifiable here was checked against `registry.npmjs.org` or probed on
`unpkg.com` on 2026-08-08 — release dates, peer ranges, which import paths ship at which major.
Where a decision hangs on a number, re-resolve it rather than trusting this text.

## Module Inventory

| Module | Covers | `_skill.md` |
|--------|--------|:-----------:|
| [version-delta](version-delta/_skill.md) | The "no Material UI v8" fact, per-major breaking changes v5→v9, the version-pinned docs trap, packages/styling/theming/slots/Grid deltas, MUI X v8 and its licensing tiers, TypeScript augmentation moves | 0.85 |

## Authoring Rules

1. **Registry over memory.** Every version, date, peer range and import-path claim cites the npm
   registry or an unpkg probe, with the date it was checked.
2. **State the major for every API.** The column that matters is *introduced*, not *removed* —
   an API that post-dates the codebase's major is as wrong as a removed one.
3. **No application-specific claims.** Which major a given product runs belongs in that consumer's
   own domain pack, never here.
