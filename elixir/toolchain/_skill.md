---
name: Toolchain & Quality Gates
path: /toolchain
tags: ["@engineering", "@dev", "@elixir", "@testing"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.8
needs_reverification: false
notes: "Framework knowledge verified by execution on a clean Ubuntu 24.04 VM on OTP 27.3.3 + Elixir 1.18.4 (2026-08-08): the setup recipe, gate exit codes, and error strings are copied from that run, not recalled. Extracted into this community pack 2026-08-12."
---

# Elixir Toolchain

## Overview

Elixir is a functional, immutable language on the Erlang VM (BEAM). For agent work on an
unfamiliar Elixir codebase it matters in three ways, in this order:

1. **Reading.** Every RCA and plan on the backend means reading Elixir. The syntax is small but the
   idioms — pattern matching in function heads, pipes, `with`, tagged tuples — mean control flow is
   often expressed by *shape* rather than by branching. Code that looks like it has no error
   handling usually has it in the function head above.
2. **Running the gates.** `mix` is build tool, task runner, package manager, and test runner in one.
   Every quality gate a playbook runs is a `mix` subcommand.
3. **Crash reading.** BEAM processes fail loudly and get restarted by a supervisor. A stack trace
   that ends in a `GenServer` terminating is a normal, well-defined event, not necessarily the bug.

This module is written to be sufficient for read-only work on day one. It does **not** make you an
Elixir engineer; it makes you able to open an unfamiliar Phoenix repo, run its checks, and explain
what a module does without guessing at syntax. Framework specifics live in [`phoenix-ecto`](../phoenix-ecto/_skill.md).

> **Verification status.** Everything in *Toolchain Setup*, *Mix*, *Testing With ExUnit* and
> *Quality Gates* below was executed on 2026-08-08 on a clean Ubuntu 24.04 VM against a
> freshly generated Phoenix app, on **OTP 27.3.3 + Elixir 1.18.4**. Exit codes
> and error strings are copied from that run, not recalled.
>
> Re-verified in review on the same date on a second, clean install of the same pair: the setup
> recipe below ran verbatim; `~c` charlist inspection, the behaviour/`@impl` and protocol examples,
> `mix credo explain` with no argument, `mix test.watch` without its dep, the credo readability
> bitmask (`4`), and the `import_config`/`config_env()` behaviour were each executed.

## Version & Compatibility

Compatibility rules worth knowing before reading a `mix.exs`:

- An **Elixir minor** (1.17 → 1.18) can add compiler warnings that break a
  `--warnings-as-errors` gate even though nothing else changed. Elixir has no 2.x; breakage arrives
  as deprecation warnings first.
- **Elixir and OTP are pinned as a pair.** A precompiled Elixir build is compiled *for* an OTP major
  (`1.18.4-otp-27`); running it on a different OTP major is unsupported and fails in confusing ways.
- `mix.exs` declares the *minimum* Elixir via `elixir: "~> 1.14"`. That is a floor, not the version
  in use — read `.tool-versions` (which both mise and asdf read) or `mise.toml` for what actually
  runs. A `Dockerfile`'s `FROM elixir:` line and a CI workflow can each pin a *different* version;
  when sources disagree, the application repo's CI is closest to the truth.

## Toolchain Setup

**Do not build Erlang from source.** The usual advice — `mise install erlang@27.3.3` — compiles OTP
from source and needs `autoconf`, `libssl-dev` and `libncurses-dev`, which are absent on the stock
runner image; it also costs 10–20 minutes. Precompiled builds exist and are what CI uses.

Verified working on a stock agent VM, **no root required**, in about two minutes:

```bash
# Erlang/OTP — precompiled, per Ubuntu release (same artefacts erlef/setup-beam uses in CI)
curl -fsSL -o otp.tar.gz \
  https://builds.hex.pm/builds/otp/amd64/ubuntu-24.04/OTP-27.3.3.tar.gz
mkdir -p ~/.local/beam/otp && tar xzf otp.tar.gz -C ~/.local/beam/otp --strip-components=1
(cd ~/.local/beam/otp && ./Install -minimal "$PWD")     # rewrites the hardcoded ROOTDIR

# Elixir — precompiled, must match the OTP major
curl -fsSL -o elixir.zip https://builds.hex.pm/builds/elixir/v1.18.4-otp-27.zip
unzip -q elixir.zip -d ~/.local/beam/elixir

export PATH="$HOME/.local/beam/otp/bin:$HOME/.local/beam/elixir/bin:$PATH"
mix local.hex --force      # Hex — package manager
mix local.rebar --force    # rebar3 — needed for Erlang deps such as telemetry
```

Available versions are listed at `https://builds.hex.pm/builds/otp/amd64/ubuntu-24.04/builds.txt`
and `https://builds.hex.pm/builds/elixir/builds.txt`.

| Piece | Command | Notes |
|-------|---------|-------|
| Version check | `elixir --version` | Prints both Elixir and the OTP it was compiled against |
| Hex | `mix local.hex --force` | Without it, `mix deps.get` cannot reach hex.pm |
| rebar3 | `mix local.rebar --force` | `telemetry` and other Erlang deps build with rebar3 |
| Phoenix generator | `mix archive.install hex phx_new --force` | Only needed to *create* apps |
| File watching | `inotify-tools` (apt, **needs root**) | Phoenix live reload only; not needed for one-shot CI runs |
| Watch-mode tests | `{:mix_test_watch, "~> 1.0", only: [:dev, :test], runtime: false}` | `mix test.watch` is **not** built in — without this dep it fails with `** (Mix) The task "test.watch" could not be found` |
| PostgreSQL | a **server**, not just `psql` | Every Ecto test suite needs one — see [`phoenix-ecto`](../phoenix-ecto/_skill.md) |

`mise` remains the right tool on a developer machine, and `.tool-versions` is the file it reads. The
recipe above is what to use on a runner where a source build is not viable.

## Language Essentials For Reading Code

**Pattern matching is assignment and branching at once.** `=` asserts a shape and binds what it
finds; a mismatch raises. Function clauses are matched top to bottom:

```elixir
def handle({:ok, member}), do: {:ok, member.id}
def handle({:error, %Ecto.Changeset{} = cs}), do: {:error, errors_on(cs)}
```

Two clauses, no `if`. If a call raises `FunctionClauseError`, the argument did not match **any**
clause — read the heads to see what was expected.

**The pipe** `|>` passes the left value as the *first* argument on the right. `a |> f(b)` is
`f(a, b)`. Pipelines read top-to-bottom and are the dominant idiom.

**`with` chains happy paths.** It matches each step and jumps to `else` on the first that fails:

```elixir
with {:ok, member} <- Clubs.fetch_member(id),
     {:ok, mandate} <- Billing.active_mandate(member) do
  {:ok, mandate}
else
  {:error, reason} -> {:error, reason}
end
```

**Tagged tuples vs exceptions.** The convention is a pair of functions: `create_member/1` returns
`{:ok, struct} | {:error, changeset}`, while `create_member!/1` raises. A trailing `!` means "raises
instead of returning an error tuple" — it is not a mutation marker. Likewise `?` means "returns a
boolean".

**Structs are maps with a compile-time shape.** `%Member{name: "Ada"}` is a map with a `__struct__`
key. `%{member | name: "Ada"}` returns a *new* struct; nothing is mutated, ever. If you are looking
for where a value was changed in place, stop — it wasn't.

**Modules, `alias`, `import`, `use`.** `alias Foo.Bar.Baz` lets you write `Baz`. `import` pulls
functions into scope unqualified. **`use` runs a macro that injects code** — `use Ecto.Schema` is why
`schema` exists as a keyword. When a function seems to come from nowhere, look at the `use` lines at
the top of the file and read that module's `__using__/1`.

**Behaviours are interfaces; `@impl true` marks an implementation.** `@behaviour GenServer` declares
that a module implements a named set of callbacks, and `@impl true` above a function says "this one
satisfies that contract" — the compiler warns if it does not. You will see them on almost every
module that matters in a Phoenix app: `GenServer`, `Plug`, `Ecto.Type`, `Phoenix.LiveView`, Bamboo
adapters. Most arrive implicitly: `use GenServer` injects `@behaviour GenServer` plus default
callbacks, which is why a `GenServer` module can define only the two callbacks it cares about.

```elixir
defmodule MyApp.Locale do
  @behaviour Plug                       # the contract: init/1 + call/2
  @impl true
  def init(opts), do: opts
  @impl true
  def call(conn, _opts), do: conn
end
```

**Protocols are the other direction — polymorphism dispatched on the data's type.** `defprotocol`
declares functions, `defimpl ... for: SomeStruct` implements them for one type. This is why `Enum`
works on maps, ranges and streams (`Enumerable`), why `to_string/1` works on your struct only if
`String.Chars` is implemented, and why encoding a struct to JSON needs
`@derive Jason.Encoder` or an explicit `defimpl`. A `** (Protocol.UndefinedError) protocol
Jason.Encoder not implemented for type Member (a struct)` is not a bug in the caller — it means
nobody implemented that protocol for that struct. (Measured on 1.18.4; older Elixir phrased the same
error as `... not implemented for %Member{}`.)

**Other things that confuse readers of unfamiliar Elixir:** atoms (`:ok`) are constants, not strings;
`@moduledoc`/`@doc` are attributes, and `@some_name value` is a compile-time constant; `defp` is
private; strings are UTF-8 binaries while `'single quotes'` are charlists (rare, and a common source
of "why is this a list of numbers").

## OTP Essentials

| Concept | What it is | Why an RCA cares |
|---------|-----------|------------------|
| Process | Isolated unit of concurrency; not an OS thread. Millions are normal | State lives *in* processes. "The value reset" often means the process restarted |
| `GenServer` | A process with `init/1`, `handle_call/3` (sync), `handle_cast/2` (async), `handle_info/2` | Most stateful services. `handle_call` has a **5 s default timeout** that surfaces as `** (exit) exited in: GenServer.call(...)` |
| Supervisor | Restarts children on crash per a strategy (`:one_for_one` restarts only the dead child) | A crash followed by normal operation is *designed*. The first crash is the bug; later ones may be the restart loop |
| Application | Unit of start/stop; `start/2` returns the supervision tree | `lib/<app>/application.ex` is the boot order in one list — read it first on an unfamiliar repo |
| "Let it crash" | Don't defensively handle the impossible; supervise instead | Missing `try/rescue` is usually intentional, not an oversight |

A real supervision tree, from a generated Phoenix app — it is a plain list, and order matters
(`Repo` before `Endpoint`, so the database is up before requests are served):

```elixir
children = [
  DemoAppWeb.Telemetry,
  DemoApp.Repo,
  {Phoenix.PubSub, name: DemoApp.PubSub},
  DemoAppWeb.Endpoint            # last: start serving only once dependencies are up
]
Supervisor.start_link(children, strategy: :one_for_one, name: DemoApp.Supervisor)
```

**Reading a crash.** A BEAM report names the process, the exit reason, and the state at exit. Work
from the innermost stack frame outward, and check whether the `(app version)` prefixes point at
application code or a dependency — the trace crosses that boundary freely.

## Mix — Build, Deps, Tasks

| Command | Purpose |
|---------|---------|
| `mix deps.get` | Resolve and fetch per `mix.exs`, writing `mix.lock` |
| `mix deps.get --check-locked` | Fail instead of silently updating the lock — the right form in CI |
| `mix deps` | List resolved dependency versions (what is actually in use) |
| `mix compile` | Compile to `_build/$MIX_ENV/` |
| `mix compile --force --warnings-as-errors` | The compile gate. `--force` matters: an incremental build recompiles nothing and passes trivially |
| `mix run priv/repo/seeds.exs` | Run a script inside the started application |
| `mix help` / `mix help <task>` | List and document tasks, including project-defined ones |

**Environments.** `MIX_ENV` is `dev` (default), `test` (set automatically by `mix test`), or `prod`.
Each gets its own `_build/` tree. Per-environment config is **not** automatic: `config/config.exs`
ends with an explicit `import_config "#{config_env()}.exs"`, and that line is the only reason
`config/test.exs` is ever read. A new `config/staging.exs` that nothing imports is silently never
evaluated.

⚠️ **Inside `config/*.exs` the macro is `config_env()`, not `Mix.env()`.** `Mix` is not available in
a release, so `Mix.env()` in a config file boots as
`** (UndefinedFunctionError) function Mix.env/0 is undefined (module Mix is not available)`.

**Compile-time vs runtime configuration** — the distinction behind most "my config change did
nothing" reports:

| File | Evaluated | Use for |
|------|-----------|---------|
| `config/config.exs`, `config/{dev,test,prod}.exs` | **Build time** | Values baked into the artefact |
| `config/runtime.exs` | **Boot time**, in the release | Anything read from the environment — the only place a release may call `System.get_env/1` |

`Application.get_env/2` reads at runtime and follows a restart. `Application.compile_env/2` bakes
the value into the compiled module — changing it needs a **recompile**, not a restart, and Elixir
raises at boot if the compiled value and the runtime value disagree.

**Custom tasks** live in `lib/mix/tasks/<name>.ex` as `defmodule Mix.Tasks.Name do use Mix.Task`.
A repo-specific `mix myapp.something` is a plain module you can read — find it there before
guessing at what it does; `mix help <task>` prints its `@shortdoc`.

**Lockfile discipline.** `mix.exs` holds *requirements* (`~> 3.14` means `>= 3.14.0 and < 4.0.0`);
`mix.lock` holds exact resolved versions with hashes. Commit both. A dependency-bump PR should show
a `mix.lock` change; a `mix.lock` diff nobody asked for means someone ran a bare `mix deps.get`
against loosened requirements.

**Aliases** in `mix.exs` compose the project's real entry points. Read them before inventing a
command — a Phoenix app conventionally defines `mix setup` and overrides `mix test` to run
`ecto.create --quiet` + `ecto.migrate --quiet` first. That override is why `mix test` works on a
fresh checkout without a manual migrate step.

## Testing With ExUnit

Tests live in `test/`, named `*_test.exs`, and are started by `test/test_helper.exs`. Support
modules (`DataCase`, `ConnCase`, fixtures) live in `test/support/` and are compiled only in the test
environment.

| Command | Use |
|---------|-----|
| `mix test` | Whole suite |
| `mix test test/demo_app/clubs_test.exs` | One file |
| `mix test test/demo_app/clubs_test.exs:42` | The test at (or above) line 42 |
| `mix test --failed` | Re-run only what failed last time — the fastest fix loop |
| `mix test --stale` | Only tests whose code changed since the last run |
| `mix test --max-failures 1` | Stop at the first failure; prints `--max-failures reached, aborting test suite` |
| `mix test --seed 0` | Disable order randomisation — use to test an order-dependence hypothesis, never as a fix |
| `mix test --only integration` / `--exclude` | Select by tag (`@tag :integration` above a test) |
| `mix test --trace` | Per-test names and timings; forces `async: false` |

`async: true` on a `use ... DataCase, async: true` runs that module concurrently with other async
modules. It is safe **only** because the Ecto SQL Sandbox gives each test its own transaction — see
[`phoenix-ecto`](../phoenix-ecto/_skill.md). Anything sharing global state (application env, a named
process, an external service) must stay synchronous.

A failure reports the assertion, a `code:` line, a `left`/`right` diff, and a stacktrace whose last
frame is the test itself. Summary lines look like `21 tests, 8 failures`, and **`mix test` exits 2**
on failure.

## Quality Gates

The composite gate, in the order that fails cheapest first:

```bash
mix format --check-formatted        # style — seconds
mix deps.get --check-locked         # lockfile integrity
mix compile --force --warnings-as-errors
mix credo --strict                  # static analysis
mix test                            # correctness
mix dialyzer                        # type/discrepancy analysis — slowest, cache the PLT
```

**Measured exit codes** (fresh `mix phx.new` app + `phx.gen.html`, this VM, 2026-08-08):

| Gate | Exit | Notes |
|------|-----:|-------|
| `mix format --check-formatted` | `1` | Prints a unified diff of what it would rewrite. `mix format` fixes it in place |
| `mix compile --force --warnings-as-errors` | `1` | `Compilation failed due to warnings while using the --warnings-as-errors option` |
| `mix credo --strict` | `6` | See bitmask below |
| `mix test` | `2` | With `21 tests, 8 failures` |
| `mix dialyzer` | `0`/`2` | `2` when discrepancies are reported |

> **A freshly generated Phoenix 1.8.9 app does not pass its own gates.** Verified: `mix format
> --check-formatted`, `mix compile --warnings-as-errors` and `mix test` all fail immediately after
> `mix phx.gen.html`, because the generator emits one over-long attribute line and *prints* the
> route to add rather than adding it. Adding `resources "/members", MemberController` to the router
> and running `mix format` took **those three** to exit `0` (measured: compile `0`, format `0`,
> test `0`). **`mix credo --strict` stayed at `6`** — its four findings are in generator-authored
> code and no fix to our code clears them. So the honest fresh-app baseline is *four of five green,
> credo red*, and `--strict` is only a viable gate on a codebase that has already been cleaned to it.
> **Do not conclude from a red gate on a fresh branch that the repo is broken** — reproduce on the
> base commit first.

**Credo** (`mix credo --strict`; config in `.credo.exs`, generated by `mix credo.gen.config`) exits
with a **bitmask**, not a count: `1` consistency, `2` design, `4` readability, `8` refactoring
opportunity, `16` warning. Our run's `6` = design (2) + readability (4), which matched the reported
"1 code readability issue, 3 software design suggestions". `mix credo explain` **requires an
argument** — either a location (`mix credo explain lib/demo_app_web.ex:91:13`) or a check module
(`mix credo explain Credo.Check.Readability.AliasOrder`); run bare it just prints the help screen.
Use `--only`/`--ignore` to scope a check. Credo is opinionated: a fresh Phoenix app emits four
`--strict` findings, so **whether `--strict` is the gate is a project decision** — read `.credo.exs`
before treating a finding as a defect.

⚠️ **Check the repo's Credo pin before trusting rule-name advice.** Old pins such as `credo ~> 1.0`
are common in long-lived codebases — same major as today's 1.7.x but many minor releases behind,
and Credo moves rule sets in minors: check names, default rule sets, and config format all differ
across that span. Advice written for 1.7 will not apply to a 1.0.x pin — read the repo's
`.credo.exs` and its `mix.lock` entry before relying on rule names.

**Dialyzer** via `dialyxir` finds type discrepancies through success typing — it reports only what it
can *prove* wrong, so a clean run is weak evidence and a finding is strong evidence. Its cost is the
PLT (Persistent Lookup Table), built once from OTP plus your dependencies and reused after.

Measured on this VM for a minimal Phoenix app: **PLT build 2m11s, whole `mix dialyzer` 3m12s wall**,
then `Total errors: 0 … done (passed successfully)`, exit `0`. A warm re-run analyses in seconds. The
PLT file itself is **5.1 MB** — modest; the cost is time, not disk.

```elixir
# mix.exs — put the PLT somewhere CI can cache
def project do
  [dialyzer: [plt_local_path: "priv/plts", plt_core_path: "priv/plts"]]
end
```

By default the PLT lands in `_build/dev/` under a name that encodes its inputs —
`dialyxir_erlang-27.3.3_elixir-1.18.4_deps-dev.plt`. **Use exactly those inputs in the CI cache
key**, plus `mix.lock`: an OTP, Elixir, or dependency change invalidates it. Without a cache the PLT
rebuilds every run and dominates CI time.

**Also worth knowing:** `mix coveralls` (ExCoveralls) for coverage — plain `mix test --cover` needs
no dependency; `mix sobelow` for Phoenix-specific security scanning. Neither is in a generated app;
both would be project-added, and their presence in `mix.exs` tells you the team already treats them
as gates.

## Ecosystem Notes — Date & Time

Two libraries appear in almost every long-lived Elixir codebase that handles schedules or billing:

- `tzdata` supplies the IANA timezone database Elixir needs for anything beyond UTC, and it
  **updates itself at runtime** by default, downloading new IANA releases — in a locked-down or
  offline environment that fails and is worth checking.
- `timex` predates modern `Calendar`/`DateTime`; new code often does not need it, but do not
  "modernise" existing use as a drive-by change.

## Gotchas

- **A red gate on a fresh Phoenix app is normal.** See *Quality Gates*. Establish the base-commit
  baseline before reporting a gate failure as a regression.
- **`mix compile` without `--force` passes trivially.** An incremental build with nothing to
  recompile emits no warnings. A `--warnings-as-errors` gate that does not also force is theatre.
- **First compile is slow, and that is not a hang.** A fresh `deps.get` + compile of a Phoenix app
  builds dozens of dependencies. Measured here for a *minimal* app: `_build` 177 MB, `deps` 97 MB,
  PLT 5.1 MB — ~280 MB. A real application with a large dependency tree and several `MIX_ENV` trees
  is a multiple of that, so budget disk generously, but the frequently-repeated "1–3 GB per app"
  figure was not reproduced at this scale. Measure before sizing a runner.
- **`mix test` needs a running PostgreSQL** for any Ecto project. With none, every test errors with
  `** (DBConnection.ConnectionError) tcp connect (127.0.0.1:5432): connection refused -
  :econnrefused` — that is a missing service, not a broken test suite.
- **Precompiled Elixir is OTP-major-specific.** `v1.18.4-otp-27` on OTP 26 or 28 is unsupported.
- **`mix local.hex` / `mix local.rebar` are per-user, not per-project.** A fresh container without
  them fails at `mix deps.get` with a message about missing Hex.
- **`String.to_atom/1` on user input leaks memory** — atoms are not garbage-collected. Seeing it on
  a request path is a finding worth raising; `String.to_existing_atom/1` is the safe form.
- **Charlists inspect as `~c"abc"`, not as a string and not as `[97, 98, 99]`.** Since Elixir 1.15
  `Inspect` renders printable charlists with the `~c` sigil — our own dialyzer run printed
  `init_plt: ~c"/tmp/.../demo_app.plt"`. A `~c"..."` where you expected a `"..."` means an Erlang
  library returned a charlist; only *non-printable* lists still show as bare integer lists. Do not
  write assertions or log greps against `[97, 98, 99]` on a 1.15+ toolchain.
- **`Logger` calls are compile-time-filtered.** Log lines can be *removed at compile time* by
  `compile_time_purge_matching`; a missing log line does not prove the branch was not taken.

## References

| Source | Use |
|--------|-----|
| https://hexdocs.pm/elixir/ | Language, `Kernel`, `Enum`, `String` — the primary reference |
| https://elixir-lang.org/getting-started/introduction.html | Guided introduction |
| https://hexdocs.pm/mix/Mix.html | Mix tasks, environments, aliases |
| https://hexdocs.pm/ex_unit/ExUnit.html | ExUnit API and CLI options |
| https://hexdocs.pm/credo/ | Credo checks, config, exit statuses |
| https://hexdocs.pm/dialyxir/ | Dialyzer via Mix, PLT configuration |
| https://builds.hex.pm/builds/otp/amd64/ubuntu-24.04/builds.txt | Precompiled OTP builds (also `.../elixir/builds.txt`) |
