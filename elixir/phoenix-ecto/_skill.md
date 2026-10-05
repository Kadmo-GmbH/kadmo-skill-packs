---
name: Phoenix & Ecto Essentials
path: /phoenix-ecto
tags: ["@engineering", "@dev", "@elixir", "@phoenix", "@database"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.8
needs_reverification: false
notes: "Verified by execution on a clean VM against a freshly generated app on Phoenix 1.8.9 / Ecto 3.14.1 / postgrex 0.22.4 / PostgreSQL 16.14 (2026-08-08); error strings are copied from that run. Bamboo claims verified against the published bamboo 2.5.0 source; set_locale behaviour read from the published library source. Extracted into this community pack 2026-08-12."
---

# Phoenix & Ecto Foundations

## Overview

Phoenix is the web framework; Ecto is the database layer. They are separate libraries that a
generated app wires together, and keeping them separate in your head is what makes a coupled
codebase tractable.

A request travels: **Endpoint** (the plug pipeline that terminates HTTP) → **Router** (matches a
path, runs a *pipeline* of plugs) → **Controller** (or LiveView) → **Context** (a plain module that
owns a slice of the domain) → **Ecto.Repo** (the only thing that talks to PostgreSQL). Responses
render through function components in `..._web/`.

The rule that matters on a long-lived, strongly coupled codebase: **the context is the
boundary.** `lib/app/` is the domain and knows nothing about HTTP; `lib/app_web/` is the web layer
and should reach the database only through a context function. Where that boundary has eroded — a
controller calling `Repo` directly, a schema reaching into web concerns — is exactly where coupling
lives and where a small change acquires a large blast radius.

Read [`toolchain`](../toolchain/_skill.md) first for language and `mix` basics.

> **Verification status.** Everything below was executed on 2026-08-08 on a clean Ubuntu 24.04 VM
> against a freshly generated Phoenix app (`mix phx.new` + `mix phx.gen.html`) on **Phoenix 1.8.9 /
> Ecto 3.14.1 / ecto_sql 3.14.0 / postgrex 0.22.4** with a real PostgreSQL 16.14. Error strings are
> copied from that run. The `set_locale` and Bamboo details are read from the actual published
> source and cited.
>
> Re-verified in review on the same date against `ecto_sql 3.14` / `postgrex 0.22`: the aggregate,
> `group_by`/`having` and `coalesce` snippets compile under `--warnings-as-errors` and build valid
> `Ecto.Query` structs, and `Sandbox.mode/2`, `start_owner!/2`, `stop_owner/1`, `checkout/1`,
> `Repo.aggregate/2..4` and `Decimal.compare/2` were each confirmed exported. The Bamboo helper list
> and the Mailjet key requirement are read from `bamboo@v2.5.0` and `bamboo_mailjet@master` source.

## Version & Compatibility

**The first thing to read on an unfamiliar Phoenix repo is its `mix.exs` and `mix.lock`.** A
library's own dependency requirements (`phoenix > 1.3.0` in some plug's manifest) say nothing about
the application that uses it — only the application's lockfile does. These are the versions this
module was verified against, and the deltas that matter most if the target codebase is older:

| Package | Verified here | Why it matters |
|---------|--------------|----------------|
| `phoenix` | 1.8.9 | Verified routes (`~p`) arrived in 1.7 and changed how paths are written; 1.8 moved layouts to function components |
| `phoenix_live_view` | 1.2.8 | 1.0 was a large API consolidation; pre-1.0 LiveView code looks materially different |
| `ecto` / `ecto_sql` | 3.14.1 / 3.14.0 | 3.x has been stable for years — the smallest risk on this list |
| `postgrex` | 0.22.4 | Driver; rarely a source of surprise |
| `gettext` | 1.0.2 | 1.0 is recent; older libraries commonly declare widened `~> 0.x or ~> 1.0` ranges to stay compatible |
| Web server | `bandit` 1.12.4 | Phoenix's default since 1.7; older apps run Cowboy |
| Mailer | `swoosh` 1.27.0 (generated default) | Long-lived apps often run Bamboo instead — see *Mail* before writing mailer code |

The generated-app defaults are not the app in front of you. Treat the table as "what current looks
like", and diff.

## Application Anatomy

```
lib/
├── demo_app/                 # THE DOMAIN — no HTTP concepts here
│   ├── application.ex        # supervision tree = boot order
│   ├── repo.ex               # Ecto.Repo — the only door to PostgreSQL
│   ├── mailer.ex
│   └── clubs.ex              # a CONTEXT: public API for one domain slice
│   └── clubs/member.ex       # a SCHEMA owned by that context
└── demo_app_web/             # THE WEB LAYER
    ├── endpoint.ex           # plug pipeline: static, session, parsers, router
    ├── router.ex             # pipelines + scopes + routes
    ├── controllers/
    ├── components/           # function components, layouts
    └── gettext.ex
```

**The supervision tree is the boot order**, and it is a plain list — read it first on an unfamiliar
repo. Note `Repo` starts before `Endpoint`, so the database is up before requests are served:

```elixir
children = [
  DemoAppWeb.Telemetry,
  DemoApp.Repo,
  {Phoenix.PubSub, name: DemoApp.PubSub},
  DemoAppWeb.Endpoint            # last
]
Supervisor.start_link(children, strategy: :one_for_one, name: DemoApp.Supervisor)
```

**Router pipelines** are named plug stacks applied by `pipe_through`. The generated `:browser`
pipeline is where session, CSRF and security headers come from — and where a locale plug like
`set_locale` would sit:

```elixir
pipeline :browser do
  plug :accepts, ["html"]
  plug :fetch_session
  plug :fetch_live_flash
  plug :put_root_layout, html: {DemoAppWeb.Layouts, :root}
  plug :protect_from_forgery
  plug :put_secure_browser_headers
end

pipeline :api do
  plug :accepts, ["json"]
end

scope "/", DemoAppWeb do
  pipe_through :browser
  get "/", PageController, :home
  resources "/members", MemberController      # 7 actions → 8 routes (update is both PATCH and PUT)
end
```

**A plug is a function or a module** that takes `%Plug.Conn{}` and returns one. Module plugs
implement `init/1` (compile time) and `call/2` (per request). A plug that calls `halt/1` **stops the
pipeline** — nothing after it runs. That is how authentication, and `set_locale`, work.

**`mix phx.routes`** prints the full routing table — method, path, controller, action — and is the
fastest way to map a URL to a controller action on an unfamiliar app.

⚠️ It will **not** print `Routes.*_path` helper names on a Phoenix 1.7+ generated app: the template
sets `use Phoenix.Router, helpers: false`, so the helper module is never generated and any
`Routes.member_path(...)` call raises `UndefinedFunctionError`. Verified routes (`~p"/members"`,
below) replaced them. An older app that still has helpers will show them — check the `use
Phoenix.Router` line before reaching for either form.

## Ecto — Schemas & Changesets

A **schema** maps a table to a struct. A **changeset** is a validation and casting pipeline that
produces either a valid change set or a collection of errors — it is *not* a database call:

```elixir
schema "members" do
  field :name, :string
  field :email, :string
  field :joined_on, :date
  timestamps(type: :utc_datetime)
end

def changeset(member, attrs) do
  member
  |> cast(attrs, [:name, :email, :joined_on])      # whitelist — unlisted keys are DROPPED
  |> validate_required([:name, :email, :joined_on])
  |> unique_constraint(:email)                      # needs a matching unique index
end
```

`cast/3` is a whitelist: a field missing from that list is silently ignored, which is the usual
explanation for "the form submitted but the value didn't save."

**Validations run in Elixir; constraints run in PostgreSQL.** `validate_required` fails before any
query. `unique_constraint` does *not* check anything by itself — it declares that if the database
raises a unique-violation on insert, Ecto should convert it into a changeset error instead of an
exception. Both surface identically to the caller. Verified error terms:

```elixir
# validate_required
[name: {"can't be blank", [validation: :required]}]

# unique_constraint, after PostgreSQL rejected the INSERT
[email: {"has already been taken",
         [constraint: :unique, constraint_name: "members_email_index"]}]
```

**`unique_constraint` without a unique index does nothing** — the insert succeeds and you get
duplicates. The index in the migration and the constraint in the changeset are two halves of one
feature; changing one without the other is a common defect.

Context functions return tagged tuples, and the bang variants raise:

```elixir
def create_member(attrs) do
  %Member{} |> Member.changeset(attrs) |> Repo.insert()   # {:ok, %Member{}} | {:error, %Ecto.Changeset{}}
end

def get_member!(id), do: Repo.get!(Member, id)            # raises Ecto.NoResultsError
```

`Ecto.NoResultsError` in a controller is not a crash to be defended against — Phoenix translates it
to a **404** by default. Adding a rescue around it usually makes behaviour worse.

## Ecto — Queries & Associations

Queries are composable data. Build them in pieces and let the context assemble:

```elixir
import Ecto.Query

def list_members(opts \\ []) do
  Member
  |> filter_active(opts[:active])
  |> order_by([m], desc: m.joined_on)
  |> Repo.all()
end

defp filter_active(q, nil), do: q
defp filter_active(q, active), do: where(q, [m], m.active == ^active)
```

**`^` pins an Elixir value into the query** — it is how parameters are bound, and it is what makes
Ecto queries safe from injection. A query built by string interpolation is a finding.

**Associations are not loaded unless you ask.** Accessing an unloaded association gives you
`#Ecto.Association.NotLoaded<association :mandates is not loaded>` — not `nil`, and not the data. It
reads as "the data is missing" when it is only unfetched:

```elixir
Repo.all(from m in Member, preload: [:mandates])          # one extra query, batched
Repo.all(from m in Member, join: x in assoc(m, :mandates), preload: [mandates: x])   # single JOIN
```

**N+1 is the default failure mode**: loading a list and then touching an association per row issues
one query per row. Ecto logs every query with its timing — a page that logs the same `SELECT` shape
dozens of times has this problem. Fix with `preload`, not with a cache.

**`Ecto.Multi` for multi-step writes.** Anything that must succeed or fail together — the shape a
billing run needs — belongs in one transaction:

```elixir
Ecto.Multi.new()
|> Ecto.Multi.insert(:member, Member.changeset(%Member{}, attrs))
|> Ecto.Multi.insert(:mandate, fn %{member: m} -> Mandate.changeset(%Mandate{}, %{member_id: m.id}) end)
|> Repo.transaction()
# {:ok, %{member: ..., mandate: ...}} | {:error, failed_step, failed_value, changes_so_far}
```

The error tuple names **which** step failed — read the second element before the third.

**Aggregates belong in the database, not in `Enum`.** A membership or billing platform is full of
"total charges this period", "members per group", "open mandates" — computing those by loading rows
and folding in Elixir pulls the whole table into memory and is the same defect class as the N+1
above:

```elixir
Repo.aggregate(Member, :count)                          # SELECT count(*)
Repo.aggregate(from(p in Payment, where: p.club_id == ^id), :sum, :amount_cents)

# grouped: one row per club, computed in PostgreSQL
from(p in Payment,
  group_by: p.club_id,
  having: sum(p.amount_cents) > 0,
  select: {p.club_id, sum(p.amount_cents), count(p.id)})
|> Repo.all()
```

Two traps. **`sum` over zero rows returns `nil`, not `0`** — `coalesce(sum(p.amount_cents), 0)` or
handle the `nil`. And **money is `:decimal`/integer cents, never float**: `Decimal` arithmetic comes
back as `%Decimal{}`, so compare with `Decimal.compare/2`, not `==`. `Repo.all |> Enum.sum` in a
billing path is a finding, not a style preference.

## Migrations

Migrations live in `priv/repo/migrations/` prefixed with a UTC timestamp, and run in filename order.

| Command | Effect |
|---------|--------|
| `mix ecto.gen.migration add_x_to_y` | Create an empty timestamped migration |
| `mix ecto.create` / `mix ecto.drop` | Create/drop the database |
| `mix ecto.migrate` | Apply pending migrations |
| `mix ecto.rollback` | Revert the **last** migration (`--step n`, `--to <version>`) |
| `mix ecto.migrations` | Show applied/pending status — run this before assuming schema state |
| `mix ecto.reset` | `drop` + `create` + `migrate` + seeds. **Never against anything shared** |

`def change` is auto-reversible for reversible operations; verified live, forward and back:

```
== Running 20260808095830 DemoApp.Repo.Migrations.CreateMembers.change/0 forward
create table members / create index members_email_index / == Migrated in 0.0s
== Running ... backward
drop index members_email_index / drop table members / == Migrated in 0.0s
```

Use `def up` / `def down` when a change is not automatically reversible (raw SQL, data
transformations). A migration that cannot be rolled back should say so rather than fail silently.

**Safety on a live database** — this is a running SaaS with member data, so a migration is a
production event:

- **Deploy order is migrate-then-deploy for additive changes, and the reverse for destructive ones.**
  Old code runs against the new schema during a rollout; a dropped or renamed column breaks the
  still-running old version. Split renames into add → backfill → switch reads → drop, across
  releases.
- `create index` **locks writes** on large tables; PostgreSQL's `CREATE INDEX CONCURRENTLY` does not,
  but cannot run inside a transaction — in Ecto that means `@disable_ddl_transaction true` plus
  `@disable_migration_lock true` and `create index(..., concurrently: true)`.
- Adding a `NOT NULL` column with a default rewrites the table on older PostgreSQL versions; on 11+
  a constant default is metadata-only. Know the server version before assuming.
- Backfills belong in their own migration or a one-off script, not mixed with DDL.

## Mail — Swoosh & Bamboo

**Phoenix 1.8 generates Swoosh; long-lived apps often run Bamboo.** The two have different APIs and
different test helpers, and carrying the idioms of one into the other produces code that compiles
against neither. Which mailer a repo uses is a one-line `mix.exs` check — make it before writing
any mail code. Everything below is Bamboo (`bamboo 2.5.0`).

Bamboo splits into a **mailer** (config, adapter) and **email structs** built by composable
functions:

```elixir
# config — required keys are ADAPTER-specific; read the adapter's handle_config/1
config :my_app, MyApp.Mailer,
  adapter: Bamboo.SomeAdapter,
  api_key: System.get_env("MAIL_API_KEY")

# building + delivering
Bamboo.Email.new_email()
|> Bamboo.Email.to("member@example.test")
|> Bamboo.Email.from("noreply@example.test")
|> Bamboo.Email.subject("Monthly invoice")
|> MyApp.Mailer.deliver_now()      # or deliver_later/1 for background delivery
```

⚠️ **Adapters validate their config at first delivery, not at boot.** An adapter's
`handle_config/1` runs when mail is sent, and several adapters require more than one credential
(Mailjet-style adapters need both an `api_key` and an `api_private_key`). Copying a single-key
config from a Mandrill or SendGrid example produces a config that compiles and boots fine and blows
up at first delivery — read the adapter's source for its mandatory keys.

**Testing mail** — configure `adapter: Bamboo.TestAdapter` in `config/test.exs`, then
`use Bamboo.Test` in the test. Verified against the published `bamboo 2.5.0` source
(`lib/bamboo/test.ex`, `lib/bamboo/adapters/test_adapter.ex`), which exports:

`assert_delivered_email/1` · `assert_delivered_email_matches/1` · `assert_email_delivered_with/1` ·
`assert_no_emails_delivered/0` · `refute_delivered_email/1` · `refute_email_delivered_with/1` ·
`refute_timeout/1`

⚠️ **`assert_no_emails_sent/0` is a trap, not a helper.** It still exists in 2.5.0 but its entire
body is `raise "assert_no_emails_sent/0 has been renamed to assert_no_emails_delivered/0"`
(`bamboo@v2.5.0:lib/bamboo/test.ex:394`), so calling it fails the test unconditionally. Use
`assert_no_emails_delivered/0`.

**`shared: true` is about *which process delivers*, not about `deliver_later`.** Plain
`use Bamboo.Test` is the default and works with `deliver_later` on its own — Bamboo's own docs say
so: "If you use `Mailer.deliver_later` without spawning another process you can use `Bamboo.Test`
with `[async: true]` and without the shared mode." You need `shared: true` only when delivery
happens in a process you did not start the test in — inside a `Task`, a `GenServer`, or a headless
browser acceptance test. And shared mode **hard-raises** on an async module ("you cannot use
Bamboo.Test shared mode with async tests"), so `shared: true` and `async: true` are mutually
exclusive, not merely discouraged.

⚠️ **Never let a test or a QA run send real mail.** Confirm the test adapter is configured before
running anything that triggers a notification path, and remember `deliver_later` moves delivery off
the calling process — a "no email sent" assertion can pass simply because the job had not run yet.

## i18n — Gettext & set_locale

In a localised Phoenix app, translations live in `priv/gettext/<locale>/LC_MESSAGES/*.po`,
extracted with `mix gettext.extract` and merged into locales with
`mix gettext.merge priv/gettext`. In code: `gettext("Member")`, `dgettext(domain, ...)`, and
`ngettext/3` for plurals. Ecto's changeset errors are translated through the app's
`error_tag`/`translate_error` helper, not by Gettext directly.

`set_locale` is a widely used Phoenix plug that resolves the locale from the URL path — but apps
equally resolve locale by session, subdomain or `Accept-Language`, so check the router before
assuming any mechanism. Read from its source (`lib/set_locale.ex`), `set_locale`'s behaviour is
more invasive than "look up a locale":

```elixir
plug SetLocale, gettext: MyApp.Gettext, default_locale: "de", cookie_key: "preferred_locale"
```

- Config keys are `gettext` and `default_locale` (both **required**), plus optional `cookie_key`
  and `additional_locales`. The old positional form `plug SetLocale, [MyApp.Gettext, "en-gb"]` is
  deprecated and emits a warning.
- It expects the locale as a **URL path segment** (`/de/members`), read from `params["locale"]`.
- **If the path has no supported locale segment, the plug rewrites the path, redirects, and calls
  `halt`.** A request to `/members` does not reach your controller — it 302s to `/de/members`.
- On success it calls `Gettext.put_locale/2` and sets `conn.assigns[:locale]`.

⚠️ **Where this plug is in the pipeline, it is the single most likely source of a confusing test or
QA failure** — so check for it *first* when an unprefixed request behaves strangely. A `ConnTest`
asserting a 200 on an unprefixed path gets a 302; a browser automation step that lands on a bare
path silently changes URL. Grep the router for `SetLocale` before writing route-level tests; if it
is there, always include the locale segment and expect redirects when you do not.

## Testing

Phoenix generates two case templates, and which one you `use` decides what you get:

| Case | For | Gives you |
|------|-----|-----------|
| `DemoApp.DataCase` | Contexts, schemas, changesets | Sandboxed repo, `Ecto.Query`/`Changeset` imported, `errors_on/1` |
| `DemoAppWeb.ConnCase` | Controllers, plugs, LiveView | Everything above plus a `%Plug.Conn{}` and `Phoenix.ConnTest` |

**The Ecto SQL Sandbox** is what makes database tests fast and parallel: each test runs inside a
transaction that is rolled back at the end, so tests never see each other's rows. Enabled in
`config/test.exs`:

```elixir
config :demo_app, DemoApp.Repo,
  pool: Ecto.Adapters.SQL.Sandbox,
  pool_size: System.schedulers_online() * 2
```

Sandboxing is **two** pieces, and a generated Phoenix 1.8 app emits both. `test/test_helper.exs`
puts the pool into manual mode once, for the whole run:

```elixir
Ecto.Adapters.SQL.Sandbox.mode(DemoApp.Repo, :manual)
```

⚠️ `Sandbox.mode/2` is **not** deprecated and is **not** optional — without that line the pool stays
in `:automatic` mode, every test writes outside an ownership transaction, and rows leak between
tests. What `start_owner!`/`stop_owner` replaced is the older per-test `Sandbox.checkout/1` +
`Sandbox.mode(Repo, {:shared, self()})` dance, which is what most examples still show:

```elixir
setup tags do
  pid = Ecto.Adapters.SQL.Sandbox.start_owner!(DemoApp.Repo, shared: not tags[:async])
  on_exit(fn -> Ecto.Adapters.SQL.Sandbox.stop_owner(pid) end)
  :ok
end
```

`shared: not tags[:async]` is the whole trick: an **async** test keeps its connection private, so it
can run concurrently; a **sync** test shares its connection with other processes, which is what lets
a controller or LiveView process see the data the test inserted. Getting this wrong produces
`DBConnection.OwnershipError` ("cannot find ownership process") — the classic symptom of a spawned
process, a `Task`, or a GenServer touching the repo from outside the test's ownership.

Fixtures are plain functions in `test/support/fixtures/` (`member_fixture/1`), generated alongside
each context. There is no factory library by default.

**You need a real PostgreSQL server** — not `postgres-client`. `mix test` is aliased to run
`ecto.create --quiet` + `ecto.migrate --quiet` first, so a missing server fails immediately with:

```
** (DBConnection.ConnectionError) tcp connect (127.0.0.1:5432): connection refused - :econnrefused
```

That is infrastructure, not a broken suite. On a runner, provide Postgres via the `docker` component
or an equivalent server.

⚠️ **`mix phx.gen.html` does not add its own route** — it *prints* the `resources` line for you to
paste. Verified: leaving it out fails 8 of 21 generated controller tests with `404`, and also fails
`mix compile --warnings-as-errors`, because Phoenix 1.8 verifies `~p` route paths at compile time
(`warning: no route path for DemoAppWeb.Router matches "/members/#{member}"`). Adding the route (plus
`mix format`) turned compile, format and test green. **`mix credo --strict` stayed at exit `6`** —
its four findings sit in generator-authored code. See
[`toolchain`](../toolchain/_skill.md) § *Quality Gates* for the full baseline.

## LiveView

LiveView renders server-side over a WebSocket: state lives in the server process, the client sends
events, the server re-renders and pushes a diff. A LiveView implements `mount/3`, `handle_event/3`,
`handle_info/2` and `render/1`, with state in `socket.assigns` updated via `assign/3`. Tests use
`Phoenix.LiveViewTest` — `live/2`, `render_click/2`, `has_element?/2` — through `ConnCase`.

> **Confirming LiveView adoption on an unfamiliar repo, in order of cost:** (1) `phoenix_live_view`
> in `mix.exs`/`mix.lock`; (2) `live "/path", SomeLive` entries in `mix phx.routes`; (3) `*_live.ex`
> / `*.html.heex` files under `lib/*_web/live/`. Any one of these settles it in under a minute. A
> React/SPA admin surface usually implies LiveView is absent or marginal — but that is an inference:
> do not plan LiveView work, and do not assume its absence, until one of the checks above has run.

## Gotchas

- **`set_locale` redirects and halts on unprefixed paths.** See *i18n*. Expect 302s in tests and
  browser runs; include the locale segment.
- **An unloaded association inspects as `#Ecto.Association.NotLoaded<association :x is not loaded>`,
  not `nil`.** It means "not fetched", never "no data".
- **`unique_constraint` without the matching unique index is a no-op** — duplicates get through.
- **`cast/3` silently drops fields not in its whitelist.** First thing to check when a submitted
  value does not persist.
- **Sandbox ownership errors mean a process outside the test touched the repo.** Either make the
  test synchronous (`async: false`, which sets `shared: true`) or give the spawned process explicit
  ownership. Do not "fix" it by disabling the sandbox.
- **`async: true` plus anything global is a flake.** Application env, named processes, and external
  services are shared across async tests even though the database is not.
- **Compile-time vs runtime config.** `config/*.exs` is evaluated at **build** time;
  `config/runtime.exs` runs at **boot** and is the only place a release may read environment
  variables. `Application.compile_env/2` bakes a value into the binary — changing it needs a
  recompile, not a restart. A setting that "won't take effect" is usually this.
- **Phoenix 1.8 verifies `~p` paths at compile time.** A route removed from the router turns every
  `~p` reference to it into a compile *warning* — which a `--warnings-as-errors` gate turns into a
  build failure. That is the feature working.
- **`Repo.get!` raising in a controller is a 404 by design.** Do not wrap it in a rescue.
- **Migration ordering is by filename timestamp, not merge order.** Two branches merged out of order
  can produce a migration that runs before the one it depends on. Check `mix ecto.migrations`.

## References

| Source | Use |
|--------|-----|
| https://hexdocs.pm/phoenix/overview.html | Phoenix guides — contexts, routing, testing, deployment |
| https://hexdocs.pm/phoenix/contexts.html | The context boundary, argued at length |
| https://hexdocs.pm/ecto/Ecto.html | Ecto — schemas, changesets, queries, `Ecto.Multi` |
| https://hexdocs.pm/ecto_sql/Ecto.Adapters.SQL.Sandbox.html | Sandbox modes and ownership errors |
| https://hexdocs.pm/ecto_sql/Ecto.Migration.html | Migrations, `concurrently:`, DDL transaction flags |
| https://hexdocs.pm/phoenix_live_view/welcome.html | LiveView lifecycle and testing |
| https://hexdocs.pm/bamboo/readme.html | Bamboo mailer, adapters, `Bamboo.Test` |
| https://hexdocs.pm/gettext/Gettext.html | Gettext extraction/merge workflow |
| https://hexdocs.pm/set_locale/ | The path-segment locale plug — read before testing any route it guards |
| [`toolchain`](../toolchain/_skill.md) | Language, `mix`, ExUnit, and the quality gates |
