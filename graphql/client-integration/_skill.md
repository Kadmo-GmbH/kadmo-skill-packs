---
name: Apollo, Codegen & Drift
path: /client-integration
tags: ["@engineering", "@dev", "@frontend", "@graphql", "@typescript", "@testing"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.75
needs_reverification: false
notes: "Package facts verified against registry.npmjs.org and the shipped @apollo/client 4.2.10 types on 2026-08-08; the drift worked example (validate silent, --check catches) was executed. Extracted into this community pack 2026-08-12."
---

# GraphQL Client Integration

## Overview

The seam between a GraphQL server and a typed frontend is where schema drift lands: the server
loosens a nullability or removes a field, every committed document stays "valid", and the
frontend's generated types quietly stop describing reality. This module covers the client half of
that seam — Apollo Client 4 and the v3 muscle memory it invalidates, mutations and cache
normalization, typed codegen and the two vendors' opposing guidance, and the three-check drift
pipeline in which the obvious check is the weakest.

The server-side counterpart for Phoenix backends — schema anatomy, error patterns, dataloader, SDL
export — is the `elixir` domain's `absinthe` module. The drift pipeline itself is server-agnostic:
it works against any exported SDL.

## Apollo Client

> **This module documents Apollo Client** — the most common client and the one with the most
> version-specific traps. If the repo uses urql, `graphql-request`, TanStack Query or bare `fetch`,
> the cache and hook material below does not apply — only the codegen and Schema Drift sections
> carry over. Check `package.json` before relying on any of it.

Current versions on npm, 2026-08-08: `@apollo/client` **4.2.10**, `urql` 5.0.3, `graphql` 17.0.2
(`latest-16` = 16.14.2), `@graphql-codegen/cli` 7.2.0, `@graphql-codegen/client-preset` 6.1.2.

Apollo Client **4** (4.0.0, released 2025-08-21) changed enough to invalidate most v3 muscle memory:

```tsx
import { gql } from '@apollo/client';
import { useQuery } from '@apollo/client/react';   // hooks moved to this subpath in v4

const GET_USER = gql`
  query GetUser($id: ID!) { user(id: $id) { id name } }
`;

function UserProfile({ userId }: { userId: string }) {
  const { loading, error, data } = useQuery(GET_USER, { variables: { id: userId } });
  if (loading) return <p>Loading…</p>;
  if (error) return <p>Error: {error.message}</p>;
  return <div>{data?.user.name}</div>;
}
```

| v3 habit | v4 reality |
|----------|-----------|
| `import { useQuery } from '@apollo/client'` | hooks live in **`@apollo/client/react`** |
| `zen-observable` | **`rxjs` is a required peer dependency** |
| `ApolloError` | removed — discriminated classes with static guards: `CombinedGraphQLErrors.is(error)`, `ServerError.is(error)`, `ServerParseError.is(error)` |
| `useQuery(..., { onCompleted, onError })` | **removed** from `useQuery`/`useSuspenseQuery` |
| `notifyOnNetworkStatusChange` defaults false | now defaults **true** |
| `useQuery<Data, Vars>(...)` generics | type the **document** with `TypedDocumentNode`; explicit hook generics become a type error under modern signatures |

`useQuery` also returns `dataState` (`"empty" | "partial" | "streaming" | "complete"`), which narrows
`data` without optional chaining. A migration codemod exists:
`npx @apollo/client-codemod-migrate-3-to-4 src`.

### Mutations

`useMutation` returns a **tuple**, not the object `useQuery` gives you — the single most common
mix-up when writing a first write against a codebase you have only read queries in:

```tsx
import { gql } from '@apollo/client';
import { useMutation } from '@apollo/client/react';
import { CombinedGraphQLErrors } from '@apollo/client';

const ADD_MEMBER = gql`
  mutation AddMember($input: AddMemberInput!) {
    addMember(input: $input) { id name }   # return the fields you changed — see the cache rules
  }
`;

function AddMemberForm() {
  const [addMember, { loading, error }] = useMutation(ADD_MEMBER, {
    // creates and deletes are the case automatic merging does NOT cover
    update(cache, { data }) {
      cache.modify({
        fields: {
          // `toReference` comes off the modifier context and yields a real cache Reference;
          // a bare `cache.identify()` string is not the same thing.
          members: (existing = [], { toReference }) => [...existing, toReference(data.addMember)],
        },
      });
    },
  });

  const onSubmit = (input: AddMemberInput) => addMember({ variables: { input } });
  if (error && CombinedGraphQLErrors.is(error)) { /* server returned `errors` */ }
  // …
}
```

Which rows of the v3→v4 table above apply here: the `@apollo/client/react` subpath and the
`ApolloError`→discriminated-classes change both do. `onCompleted`/`onError` were removed from
`useQuery`/`useSuspenseQuery` but **remain** on `useMutation` — verified in the shipped
`@apollo/client@4.2.10` types (`react/hooks/useMutation.d.ts`, where both are still declared, and
where `onCompleted` no longer appears for the query hooks). It is the one place that habit still
works, so do not let a blanket search-and-replace strip them here.

On an Absinthe server a mutation is just a `mutation do … end` root beside `query do … end`,
resolving the same way; whether its validation failures come back as top-level `errors` or as a
typed payload is the server's "errors as errors" vs "errors as data" choice (see the `elixir`
domain's `absinthe` module § *Errors & Changesets*), and it is the thing to check before writing
the form's error handling.

### Typed codegen — and a live disagreement

```ts
// codegen.ts
import type { CodegenConfig } from '@graphql-codegen/cli';

const config: CodegenConfig = {
  schema: 'schema.graphql',                     // the SDL exported from Absinthe
  documents: ['src/**/*.ts', '!src/gql/**/*'],  // must exclude the output dir
  ignoreNoDocuments: true,
  generates: { './src/gql/': { preset: 'client' } },
};
export default config;
```

```tsx
import { graphql } from './gql';
import { useQuery } from '@apollo/client/react';

const UserById = graphql(/* GraphQL */ `
  query UserById($id: ID!) { user(id: $id) { id name role } }
`);

const { data } = useQuery(UserById, { variables: { id } });
//      ^? UserByIdQuery | undefined — inferred, no generics
```

> **The two vendors disagree, and you must not silently pick a side.** The Guild calls `client-preset`
> *"the recommended approach"*. Apollo's own docs say: *"We do not recommend using the client preset
> with Apollo Client apps"* — it adds runtime bundle size and its fragment masking is incompatible
> with Apollo's data masking. Apollo recommends `typescript` + `typescript-operations` +
> `typed-document-node` instead. If you use client-preset with Apollo, `fragmentMasking: false` is
> mandatory. **Match whatever the repo already does**; raise the trade-off in the PR rather than
> converting a codebase mid-ticket.

`@graphql-codegen/typescript-react-apollo` is functionally dead — its generated hooks are not
compatible with Apollo Client 4.

### Cache behaviour worth knowing before you cause a bug

- Objects are normalized by `__typename` + `id` → `Todo:5`. **`__typename` is added automatically;
  `id` is not — you must select it.** Without an id the object is stored inside its parent and cannot
  be shared or updated independently.
- Override with `typePolicies: { Product: { keyFields: ["upc"] } }`; `keyFields: false` embeds.
- **A mutation must return the fields it changed.** Apollo merges the response over the cached entity
  by key; fields you omit are simply not updated. Getting this right removes most need for manual
  cache work.
- **Automatic merging does not cover list membership.** Creates and deletes still need an `update`:
  `cache.evict({ id: cache.identify(data.deleteProduct) })` then `cache.gc()`.
- `fetchPolicy`: `cache-first` (default), `cache-and-network`, `network-only` (skips cache, still
  writes it), `no-cache` (neither reads nor writes), `cache-only`, `standby`.
- Two messages that mean you broke the cache: *"Cache data may be lost when replacing the X of a Y
  object"* (arrays have no `__typename` — supply a `merge`) and *"Missing field 'X'"* (select `id`).
  Inspect with `client.cache.extract()`.

### Subscriptions — both paths are stale, pick deliberately

If real-time is ever in scope: the path Absinthe's guides still point at, `@absinthe/socket` /
`@absinthe/socket-apollo-link`, was **last published 2019-02-20** and has had two commits in five
years. It depends on `apollo-link@1.2.5` (the Apollo *2* link class), pins `graphql` to exactly
`14.0.2`, and uses `zen-observable` — so it **cannot work with Apollo Client 4** and pins you to
Apollo 3.

The less-bad option is `absinthe_graphql_ws` on the server (now in the official
`absinthe-graphql` org) with the standard `graphql-ws` client, which Apollo 4 supports via
`@apollo/client/link/subscriptions`. Its own last hex release is 2022-07-29 and it has 11 open PRs —
so this is choosing between two stale things, not choosing a healthy one. Say that out loud in any
proposal rather than presenting either as maintained.

## Schema Drift

Drift between the schema and its clients is *mechanically* detectable, because the server can
export its schema:

```bash
mix absinthe.schema.sdl --schema MyAppWeb.Schema schema.graphql   # diffable SDL — prefer this
mix absinthe.schema.json --schema MyAppWeb.Schema --pretty        # introspection JSON for tooling
```

Both tasks compile and boot the schema themselves, so they run in CI without a live server.

**Three checks. Run all three — the obvious one is the weakest.**

| Check | Catches | Blind to |
|-------|---------|----------|
| `graphql-inspector validate <schema> <documents>` | removed fields, newly-required args, fields the documents ask for that no longer exist | **nullability changes — exits 0** |
| `graphql-codegen --check` | anything that changes the generated TS shape, including nullability | drift that does not reach a generated type; needs generated output committed |
| `graphql-inspector diff <old.graphql> <new.graphql>` | any schema-level change, classified breaking / dangerous / non-breaking | what *clients actually use* — it flags changes no document queries |

The three answer different questions — *"are our documents still legal?"*, *"did our generated types
move?"*, and *"what changed in the schema at all?"* — so a pipeline wants all three.

Worked example, and the reason the middle row exists. A server loosens `User.name` from `String!` to
`String`:

```
graphql-inspector validate …          → "All documents are valid"        exit 0   ← silent
graphql-codegen (plain run)           →                                  exit 0   ← silent
graphql-codegen --config codegen.ts --check
                                      → "stale files detected"           exit 1   ← caught
graphql-inspector diff old new        → "Field User.name changed type
                                         from String! to String"         exit 1   ← caught
```

Both silent tools are *correct*: the document really is still valid, and a plain codegen run just
rewrites its output without comparing. Meanwhile the generated type quietly goes from
`name: string` to `name: string | null`, and the frontend starts receiving `null` where its types
promise a string. **Note the distinction between a plain `graphql-codegen` run and
`graphql-codegen --check`** — only the second is a gate. (`--check` is undocumented in `--help` for
cli 7.2.0 but functional; if the generated output is Prettier-formatted afterwards it will report
stale forever, in which case gate on `git diff --exit-code` instead.)

A CI recipe that follows from this: export SDL on the server side, commit it, then on the client
validate documents against it, re-run codegen with `--check`, and diff the SDL against the base
branch to classify the change.

> **Pin `graphql@16`.** `@graphql-codegen/cli` accepts graphql 17, but `@graphql-inspector/*` accepts
> only `^14 || ^15 || ^16`, and `@graphql-eslint/eslint-plugin` accepts `^16` exactly. Adopting
> graphql 17 silently removes two of the three checks above.

Classification rules worth knowing when reading a diff: removing a field is **breaking even if it was
deprecated first**; adding a non-null argument *without* a default is breaking, *with* a default is
merely dangerous; adding an enum value is dangerous on an existing enum but harmless on a new one.

Config keys that silently disable the gate, and must never appear in CI:
`skipDocumentsValidation`, `allowPartialOutputs: true`. Use `@graphql-inspector/cli` rather than
`@graphql-inspector/ci`, which trails it by a major and nine months (`ci` 5.0.7, 2025-11-12 vs `cli`
6.0.8, 2026-04-30).

> **One caveat to verify before trusting a freshness gate:** `mix absinthe.schema.sdl` renders via
> `inspect(blueprint, pretty: true)`, and its output determinism across builds is unverified. If a
> `--check` or `git diff --exit-code` gate flaps with no real change, normalise or sort the SDL
> before committing it.

## References

| Source | Use |
|--------|-----|
| [registry.npmjs.org](https://registry.npmjs.org/) | Every version, publish date and peer/dependency range asserted here — `@apollo/client`, `graphql`, `@graphql-codegen/*`, `@graphql-inspector/*`, `@absinthe/socket*` — re-resolve rather than trusting the numbers; they are true as of `last_verified` |
| [Apollo Client 4 migration](https://www.apollographql.com/docs/react/migrating/apollo-client-4-migration) · [caching](https://www.apollographql.com/docs/react/caching/overview) | v3→v4 breaks; normalization and cache updates |
| [GraphQL Code Generator client-preset](https://the-guild.dev/graphql/codegen/plugins/presets/preset-client) · [Apollo's codegen guidance](https://www.apollographql.com/docs/react/development-testing/graphql-codegen) | The two opposing official positions |
| [graphql-inspector](https://the-guild.dev/graphql/inspector) | `validate` and `diff` for drift detection |
| [GraphQL spec, September 2025](https://spec.graphql.org/September2025/#sec-Errors) | Error shape and non-null propagation |
