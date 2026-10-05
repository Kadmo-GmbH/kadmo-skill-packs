---
name: Elixir Foundations
domain: elixir
path: /
tags: ["@engineering", "@dev", "@elixir", "@phoenix", "@graphql", "@testing"]
learned: "2026-08-08"
last_verified: "2026-08-12"
confidence: 0.8
---

# Elixir Foundations

Framework-level Elixir knowledge for agents working an unfamiliar Elixir/Phoenix codebase: reading
the language, running the quality gates, and working the web, data and GraphQL layers without
guessing at syntax or idiom. Everything execution-level was verified on a clean Ubuntu 24.04 VM
(OTP 27.3.3 + Elixir 1.18.4, Phoenix 1.8.9, Ecto 3.14.1, absinthe 1.11.0) on 2026-08-08 — exit
codes and error strings are copied from those runs, not recalled.

This pack is **generic by design**: framework and ecosystem facts only, no application-specific
claims. Pair it with a per-application domain pack that records what the target codebase actually
pins and does — which mailer, which locale mechanism, whether LiveView is present, which Credo pin.
The modules here tell you what to check; the application pack records the answers.

## Module Inventory

| Module | Covers | `_skill.md` |
|--------|--------|:-----------:|
| [toolchain](toolchain/_skill.md) | Language reading essentials, OTP, `mix`, ExUnit, quality gates, runner setup with precompiled builds | 0.8 |
| [phoenix-ecto](phoenix-ecto/_skill.md) | Request path and contexts, schemas/changesets, queries, migrations, sandbox testing, mail, i18n, LiveView | 0.8 |
| [absinthe](absinthe/_skill.md) | GraphQL schema anatomy, Phoenix integration, errors & changesets, dataloader/N+1, SDL export | 0.75 |

Read `toolchain` before `phoenix-ecto`; read `absinthe` only when the codebase serves GraphQL. The
client side of a GraphQL seam (Apollo, codegen, drift gating) is the `graphql` domain's
`client-integration` module.

## Authoring Rules

1. **Cite a source for every non-obvious claim** — hexdocs, a registry manifest, a spec, or a
   measured run. "Verified" means executed or read from published source, and the frontmatter's
   `last_verified` says when.
2. **Version-pin what is version-sensitive.** Name the exact version a claim was verified against.
   Model training data skews old; unmarked version drift is the likeliest way this content
   misleads.
3. **Mark uncertainty.** Where behaviour varies by project configuration, say so and state what
   would confirm it — do not present a guess as fact.
4. **No application-specific claims.** Facts about one company's repo, versions, or process belong
   in that consumer's own domain pack, never here.
