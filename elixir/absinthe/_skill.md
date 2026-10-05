---
name: Absinthe GraphQL
path: /absinthe
tags: ["@engineering", "@dev", "@elixir", "@graphql"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.75
needs_reverification: false
notes: "Compile-level claims verified against absinthe 1.11.0 (2026-08-08) — the two compile-time traps and the dataloader helper arities were executed; the error-metadata behaviour was read from the published source. Extracted into this community pack 2026-08-12."
---

# Absinthe GraphQL

## Overview

Absinthe is the de-facto GraphQL server for Phoenix. This module covers the schema type system,
Phoenix integration, error handling (including Ecto changesets), and the N+1/dataloader story —
enough to read an existing schema, answer "will this query succeed?", and extend a schema without
breaking its conventions.

The client side of the seam — Apollo, typed codegen, and schema-drift gating — lives in the
`graphql` domain's `client-integration` module; read the two together when a change crosses the
server/client boundary. Read [`phoenix-ecto`](../phoenix-ecto/_skill.md) first for router, context
and changeset basics.

Current versions on hex.pm as of 2026-08-08: **`absinthe` 1.11.0** (2026-06-04), `absinthe_phoenix`
2.0.5, `absinthe_plug` 1.5.10, `dataloader` 2.0.2 — all MIT.

## Schema Anatomy

The root schema and the type modules it imports live in **separate files** — this layout is what
compiles, and the next note explains why it is not merely a style choice:

```elixir
# lib/my_app_web/schema/account_types.ex
defmodule MyAppWeb.Schema.AccountTypes do
  use Absinthe.Schema.Notation      # type modules use Notation, NOT Absinthe.Schema

  object :user do
    field :id, non_null(:id)
    field :name, :string
    field :posts, list_of(:post) do
      arg :limit, :integer, default_value: 10
      resolve &MyAppWeb.Resolvers.Content.list_posts/3
    end
  end
end
```

```elixir
# lib/my_app_web/schema.ex
defmodule MyAppWeb.Schema do
  use Absinthe.Schema

  import_types Absinthe.Type.Custom      # :datetime, :date, :decimal — not built in by default
  import_types MyAppWeb.Schema.AccountTypes

  query do
    @desc "Get a user"
    field :user, :user do
      arg :id, non_null(:id)
      resolve &MyAppWeb.Resolvers.Accounts.find_user/3
    end
  end
end
```

> **Two compile-time traps here, both verified by compiling against absinthe 1.11.0.**
>
> 1. **`object` only exists inside a notation module.** `object`, `field`, `arg`, `list_of` and
>    `non_null` are macros injected by `use Absinthe.Schema` / `use Absinthe.Schema.Notation`. Written
>    at the top level of a file — outside any `defmodule` — you get
>    `error: undefined function object/2 (there is no such import)`.
> 2. **`import_types` needs the type module to already be compiled.** Absinthe resolves it in the root
>    schema's `__after_compile__`, so if you collapse the two modules above into one file with the
>    schema first, compilation dies with `Could not load module … AccountTypes. It returned reason:
>    nofile` followed by `In field User, :user is not defined in your schema`. Separate files let the
>    compiler order them for you; within a single file the types module must come **first**.
>
> If you ever see "`:x` is not defined in your schema" and the type is plainly right there, suspect
> trap 2 before you start doubting the type.

**The resolver contract.** 3-arity `(parent, args, resolution)` or 2-arity `(args, resolution)`.
Returns `{:ok, value}` or `{:error, reason}`. Arguments arrive **atom-keyed**, already coerced, with
unknown args culled — and `non_null` args are validated *before* the resolver runs, so a missing
required arg never reaches your code.

```elixir
def find_user(_parent, %{id: id}, %{context: %{current_user: _}}) do
  case Accounts.find_user(id) do
    nil  -> {:error, "User ID #{id} not found"}
    user -> {:ok, user}
  end
end
def find_user(_parent, _args, _resolution), do: {:error, "Access denied"}
```

On a non-root field, argument 1 is the **parent struct** — that is how `user.posts` resolves against
the user it hangs off.

> **The naming adapter is the first thing to check when asking "will this query succeed?"** Absinthe's
> default `Absinthe.Adapter.LanguageConventions` means the schema is written in `snake_case` while
> documents and results use `camelCase` — *including names in errors*. So a frontend query naming
> `firstName` matches a schema field declared `first_name`, and a mismatch you "see" may be an adapter
> artefact rather than real drift.
>
> The rule is *underscore-to-camel*, and it applies to digits too, which is where it surprises:
> `iso_639_1` becomes `iso6391`, not `iso639_1`. **Verify rather than assume** — the adapter is
> overridable (`Absinthe.Adapter.Underscore` keeps `snake_case`; `Passthrough` disables translation
> and breaks introspection), and it can be set per-run. Grep the schema module and the `Absinthe.Plug`
> options for `adapter:`; if nothing sets it, the default above holds. Fastest ground truth of all:
> dump the SDL (see Schema Drift) and read the names the server actually publishes.

### Answering "will this query succeed?"

The module's acceptance question, as a checklist. Work it against the SDL dump, not the Elixir source
— the SDL is what the client actually sees.

1. **Names** — apply the adapter rule above before concluding a field is missing.
2. **Field exists on that type**, at every level of the selection. Leaf fields must be scalars;
   object fields must have a selection set.
3. **Required arguments** — any `non_null` arg without a default must be supplied. These are
   validated *before* resolvers run, so a miss is a request error and yields no `data` at all.
4. **Argument types** — unknown args are culled rather than erroring, so a typo'd optional arg fails
   silently rather than loudly.
5. **Scalars are declared** — only `Int`, `Float`, `String`, `ID`, `Boolean` exist by default. A
   `DateTime` or `Decimal` field requires `import_types Absinthe.Type.Custom` server-side.
6. **Authorization** — a resolver reading `context.current_user` returns an error, not data, when the
   request is unauthenticated. A query that "works in GraphiQL" and fails from the app is usually this.
7. **Nullability** — check what happens when a field *does* fail; see the propagation rule below.
8. **Limits** — a deeply nested or wide query can be rejected by `max_complexity` or `token_limit`
   if the server enables them.

GraphiQL (mounted at `/graphiql` in dev) settles 1–5 interactively and is the fastest route to an
answer when you have the app running.

Other type-system pieces you will meet: `enum` (with `value :red, as: "r"` to control the internal
Elixir term), `input_object` (the legal input types are scalars, **enums** and input objects, plus
any list/non-null wrapping of those — GraphQL spec §3.4.3 `IsInputType`; objects, interfaces and
unions are output-only), `interface` +
`interfaces [...]`, and `union` with a `resolve_type` callback. Built-in scalars are only
`Int`, `Float`, `String`, `ID`, `Boolean` — dates and decimals require the `Absinthe.Type.Custom`
import above, which is a common "why is `:datetime` unknown" stumble.

## Phoenix Integration

```elixir
# router.ex
pipeline :graphql do
  plug MyAppWeb.Context             # builds the auth context
end

scope "/api" do
  pipe_through :graphql
  forward "/", Absinthe.Plug, schema: MyAppWeb.Schema
end

if Mix.env() == :dev do
  forward "/graphiql", Absinthe.Plug.GraphiQL, schema: MyAppWeb.Schema
end
```

> **`forward` syntax differs between routers.** `Phoenix.Router` takes options inline as above;
> plain `Plug.Router` needs `to:` and `init_opts:`. Copying the wrong one is a classic breakage.

Order matters: if you accept anything beyond `application/graphql`, plug `Absinthe.Plug` **after**
`Plug.Parsers`, and add `Absinthe.Plug.Parser` to the parsers list.

Authentication is a plug that stuffs the context; the context is set once at `Absinthe.run` and
**cannot be modified during execution**:

```elixir
defmodule MyAppWeb.Context do
  @behaviour Plug
  import Plug.Conn

  def init(opts), do: opts

  def call(conn, _), do: Absinthe.Plug.put_options(conn, context: build_context(conn))

  defp build_context(conn) do
    with ["Bearer " <> token] <- get_req_header(conn, "authorization"),
         {:ok, user} <- authorize(token) do
      %{current_user: user}
    else
      _ -> %{}
    end
  end
end
```

Convention worth copying: when there is no user, **omit the `:current_user` key** rather than setting
it to `nil`, so resolvers can pattern-match `%{context: %{current_user: user}}` and fall through to a
deny clause.

Everything in Absinthe is middleware — `resolve` is itself sugar for `middleware Absinthe.Resolution`.
Apply cross-cutting behaviour with the schema's `middleware/3` callback:

```elixir
def middleware(middleware, _field, %{identifier: :mutation}) do
  middleware ++ [MyApp.Middlewares.HandleChangesetErrors]
end
def middleware(middleware, _field, _object), do: middleware
```

**All middleware on a field always run**, so pattern-match on state if you only want to act on some
outcomes. Subscriptions, if present, need `absinthe_phoenix`, `{Absinthe.Subscription, Endpoint}` in
the supervision tree, `use Absinthe.Phoenix.Endpoint`, and socket context set in `connect/2` rather
than by a plug.

## Errors & Changesets

### The wire shape

Per the GraphQL spec (current release **September 2025**), an error carries `message` (required),
plus `locations`, `path` and `extensions`. Two classes behave differently:

- **Request errors** (parse / validation / variable coercion): `data` **must not be present**;
  execution is halted. Absinthe emits `%{errors: [...]}` with no `:data` key.
- **Execution errors** (a resolver failed): partial `data` *plus* `errors`, and the error carries a
  `path` so the client can tell a real `null` from a failure.

Absinthe omits the `:errors` key entirely when there are none, and de-duplicates errors.

> **Non-null propagation is the behaviour that surprises people.** An error on a `non_null` field
> nulls the **nearest nullable ancestor**; if every ancestor is non-null, `data` itself becomes
> `nil` — one failed leaf can blank the entire response. The error's `path` still points at the
> original position even though that position no longer appears in `data`. When a frontend reports
> "the whole query returned null", this is the first thing to check, and the fix is usually schema
> nullability, not the resolver.
>
> **But relaxing `String!` to `String` is itself a client-breaking change** — it is exactly the drift
> that `graphql-inspector validate` and a plain codegen run are blind to (see Schema Drift). If you
> make a field nullable to stop the bubbling, re-export the SDL, re-run codegen, and expect the
> frontend types to gain a `| null` that call sites must now handle.

> **Gotcha, verified in the v1.11.0 source:** extra metadata on an error map lands at the **top level
> of the error object, not under `extensions`**, by default. `{:error, message: "Unknown user", code: 21}`
> produces `%{message: ..., code: 21, path: ..., locations: ...}`. The spec-compliant behaviour exists
> (`spec_compliant_errors`) but is not a documented `Absinthe.run/3` option and is not enabled in the
> default pipeline. Do not promise a client that custom codes arrive under `extensions`.

Accepted `{:error, ...}` values: a string, a list of strings, a keyword list with `:message` plus
metadata, a map containing `:message`, or a list mixing those.

### Changesets → errors

Ecto gives you `{:error, %Ecto.Changeset{}}`; GraphQL needs strings or maps with `:message`. Two
patterns, and the choice is architectural.

**Pattern A — errors as errors** (the documented Absinthe approach). A post-resolution middleware
rewrites the raw changeset parked in `resolution.errors`:

```elixir
defmodule MyApp.Middlewares.HandleChangesetErrors do
  @behaviour Absinthe.Middleware

  def call(resolution, _) do
    %{resolution | errors: Enum.flat_map(resolution.errors, &handle_error/1)}
  end

  defp handle_error(%Ecto.Changeset{} = changeset) do
    changeset
    |> Ecto.Changeset.traverse_errors(fn {msg, opts} ->
      # interpolate %{count} etc. — the Absinthe guide's version omits this and leaks the template
      Regex.replace(~r"%{(\w+)}", msg, fn _, key ->
        opts |> Keyword.get(String.to_existing_atom(key), key) |> to_string()
      end)
    end)
    |> Enum.map(fn {field, msgs} -> "#{field}: #{Enum.join(msgs, ", ")}" end)
  end

  defp handle_error(error), do: [error]
end
```

Attach it to mutations only, via the `middleware/3` callback shown above.

Why this works: a resolver returning `{:error, %Ecto.Changeset{}}` — exactly what `Repo.insert/1`
hands back — parks the **raw struct** in `resolution.errors`. A changeset is not one of the error
values Absinthe knows how to serialise, so without this middleware it fails at result-building. The
middleware must therefore run *after* resolution, which is what `middleware ++ [...]` (append, not
prepend) achieves. Prepend it and it runs before the resolver, sees an empty error list, and does
nothing.

**Pattern B — errors as data** (a typed mutation payload). The mutation returns an object with
`successful`, `messages` and `result` fields; validation failures become queryable, non-null-safe
*data*. `absinthe_error_payload` (BSD-3) implements it, and states the rationale plainly: messages
from bad user input are **data**, whereas querying a field that does not exist is an **error**.

Pattern B costs a wrapper type per mutation but buys three things that matter here: validation errors
survive client codegen as a real type, they carry per-field structure instead of a flattened string,
and they do not trigger the null-bubbling above. **Look at which pattern the codebase already uses
and match it** — mixing the two in one schema is the actual failure mode.

## Performance

The N+1 shape: resolve a list of N posts, then resolve `author` per post with `Repo.one` ⇒ N+1
queries. Dataloader is the current answer.

```elixir
# 1. a source per context module — narrow and authorize here
defmodule MyApp.Blog do
  import Ecto.Query          # `from` is a macro — without this the module does not compile
  alias MyApp.Blog.Post      # so the `Post` in the heads below matches the real module, not `Elixir.Post`

  def data, do: Dataloader.Ecto.new(MyApp.Repo, query: &query/2)

  def query(Post, %{has_admin_rights: true}), do: Post
  def query(Post, _), do: from(p in Post, where: is_nil(p.deleted_at))
  def query(queryable, _), do: queryable
end

# 2. field usage — in the type module. Note this comes BEFORE the schema module in a single
#    file, for the `import_types` ordering reason in Schema Anatomy above.
defmodule MyAppWeb.Schema.BlogTypes do
  use Absinthe.Schema.Notation
  import Absinthe.Resolution.Helpers      # every arity you use — see the bullets below

  object :post do
    field :author, :user, resolve: dataloader(MyApp.Blog)
  end

  object :user, do: field(:id, non_null(:id))
end

# 3. the two schema callbacks — in the root schema module
defmodule MyAppWeb.BlogSchema do
  use Absinthe.Schema
  import_types MyAppWeb.Schema.BlogTypes

  def context(ctx) do
    loader = Dataloader.new() |> Dataloader.add_source(MyApp.Blog, MyApp.Blog.data())
    Map.put(ctx, :loader, loader)
  end

  def plugins, do: [Absinthe.Middleware.Dataloader] ++ Absinthe.Plugin.defaults()

  query do
    field :posts, list_of(:post)
  end
end
```

- **`dataloader` is not auto-imported** — `batch` and `async` are, but dataloader is an optional
  dependency, so the explicit `import Absinthe.Resolution.Helpers` is required. Forgetting it is the
  usual first error. **Import every arity you use:** the helper exists at `dataloader/1`, `/2` and
  `/3` (absinthe 1.11.0 source), so an `only: [dataloader: 1]` list makes the override form below
  fail with `undefined function dataloader/3`. A bare `import Absinthe.Resolution.Helpers` avoids the
  whole problem.
- `dataloader(Blog)` infers the association from the **field name**; pass `dataloader(Blog, :posts,
  args: %{deleted: false})` to override, or a key function to clamp client-supplied values
  (`max(min(limit, 20), 0)`).
- Putting the authorization filter in `query/2` is the point: the source, not the resolver, has final
  say over data access.
- `Absinthe.Middleware.Batch` is the older mechanism and still works, but its documented drawbacks —
  one batch per field, and the need to thread `self()` through batch keys for the concurrent test
  sandbox — are why dataloader replaced it.

Depth and cost limits are opt-in:

```elixir
plug Absinthe.Plug, schema: MyAppWeb.Schema, analyze_complexity: true, max_complexity: 50
```

Every field costs 1 by default; a `complexity` function receives the args and the summed child
complexity, so `arg :limit` fields should multiply. On breach, resolution is skipped and an error
returned. A separate `token_limit` option caps the lexer (`"Token limit exceeded"`).

## Exporting The Schema

The server can export its schema, so drift between the schema and its clients is *mechanically*
detectable:

```bash
mix absinthe.schema.sdl --schema MyAppWeb.Schema schema.graphql   # diffable SDL — prefer this
mix absinthe.schema.json --schema MyAppWeb.Schema --pretty        # introspection JSON for tooling
```

Both tasks compile and boot the schema themselves, so they run in CI without a live server. The CI
recipe that follows: export the SDL server-side, commit it, and let the client pipeline validate
documents against it, re-run codegen with `--check`, and diff the SDL against the base branch. The
client half of that pipeline — and why the obvious check is the weakest of the three — is in the
`graphql` domain's `client-integration` module.

> **One caveat to verify before trusting a freshness gate:** `mix absinthe.schema.sdl` renders via
> `inspect(blueprint, pretty: true)`, and its output determinism across builds is unverified. If a
> `--check` or `git diff --exit-code` gate flaps with no real change, normalise or sort the SDL
> before committing it.

## References

| Source | Use |
|--------|-----|
| https://hexdocs.pm/absinthe/overview.html | Schema, middleware, context, errors, dataloader, complexity, testing |
| https://hexdocs.pm/absinthe/middleware-and-plugins.html | The changeset-error middleware pattern |
| https://hexdocs.pm/absinthe/dataloader.html | N+1 avoidance and why dataloader beat `Batch` |
| https://hexdocs.pm/absinthe/batching.html | The older `Batch` middleware and its drawbacks |
| https://hexdocs.pm/absinthe/adapters.html | `snake_case` ↔ `camelCase` — read before diagnosing a "missing" field |
| https://spec.graphql.org/September2025/#sec-Errors | Error shape and non-null propagation |
| https://hexdocs.pm/absinthe_error_payload/readme.html | The typed-payload error pattern (Pattern B) |
