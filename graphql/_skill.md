---
name: GraphQL Client Integration
domain: graphql
path: /
tags: ["@engineering", "@dev", "@frontend", "@graphql", "@typescript", "@testing"]
learned: "2026-08-08"
last_verified: "2026-08-12"
confidence: 0.75
---

# GraphQL Client Integration

The seam between a GraphQL server and a typed frontend is where schema drift lands: the server
loosens a nullability or removes a field, every committed document stays "valid", and the
frontend's generated types quietly stop describing reality. This pack covers the client half of
that seam and the drift pipeline that makes the seam testable.

Package facts were verified against `registry.npmjs.org` and the shipped `@apollo/client` 4.2.10
types on 2026-08-08; the drift worked example (document validation silent, `--check` catches) was
executed, not recalled.

## Module Inventory

| Module | Covers | `_skill.md` |
|--------|--------|:-----------:|
| [client-integration](client-integration/_skill.md) | Apollo Client 4 vs v3 muscle memory, mutations and cache normalization, typed codegen and the two vendors' opposing guidance, the three-check schema-drift pipeline, subscription-path staleness | 0.75 |

For Phoenix backends the server-side counterpart — schema anatomy, error patterns, dataloader, SDL
export — is the `elixir` domain's `absinthe` module. The drift pipeline itself is server-agnostic.

## Authoring Rules

1. **Name the client and its major.** Apollo 3 and 4 differ enough that unmarked advice is wrong
   half the time; urql and fetch-based clients share only the codegen and drift material.
2. **Registry over memory** for every version and peer-range claim, with the date it was checked.
3. **No application-specific claims.** Which client, preset and error convention a given product
   uses belongs in that consumer's own domain pack, never here.
