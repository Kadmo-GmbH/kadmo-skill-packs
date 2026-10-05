---
name: Version Migration Map
path: /version-delta
tags: ["@engineering", "@dev", "@frontend", "@react", "@typescript"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.85
needs_reverification: false
notes: "Registry-verified: every version, release date, peer range and import-path claim was checked against registry.npmjs.org or probed on unpkg.com on 2026-08-08. Re-resolve rather than trusting the numbers where a decision hangs on them. Extracted into this community pack 2026-08-12."
---

# MUI Version Migration Map — Core v5→v9 and MUI X v8

## Overview

MUI is where model training data most likely betrays a React agent: the library has shipped
multiple majors since the v5 era that training corpora over-represent, and it renamed or removed
exactly the APIs a model reaches for by reflex (`Grid item xs`, `componentsProps`, `makeStyles`,
`InputProps`). This module is a **delta**, not a MUI tutorial: what changed at each major, which
docs to trust for which version, and the traps that compile cleanly and fail at runtime.

**Read the codebase's lockfile before writing any MUI code.** There is no single dialect valid
across v5, v6 and v7 — the Grid API alone differs in all three — so the first action on any MUI
ticket is `package.json`/lockfile: the `@mui/material` major, any `@mui/x-*` packages and their
licensing tier, the React major, and Emotion vs styled-components.

## There Is No Material UI v8

**Material UI (MUI Core) has never published a version 8.** Verified against the npm registry on
2026-08-08 — `@mui/material` stable majors are **5, 6, 7, 9**. Not one 8.x release exists, not even
a prerelease; `GET registry.npmjs.org/@mui/material/8.0.0` returns HTTP 404. The same 5/6/7/9 gap
holds across `@mui/system`, `@mui/icons-material`, `@mui/styled-engine`, `@mui/utils` and
`@mui/material-nextjs`.

MUI say so themselves, in the v9 launch post:

> "So for v9, we're realigning: Material UI moves from v7 straight to v9 (**there is no Material UI
> v8, like there is no v2**), in step with MUI X v9, restoring a single shared major for the suite
> for the first time since that split."
> — [Introducing Material UI and MUI X v9](https://mui.com/blog/introducing-mui-v9/), 2026-04-08

The corresponding GitHub milestone description is blunter: *"We skip v8 to align with MUI X next
major version"* ([milestone 52](https://github.com/mui/material-ui/milestone/52)).

**Only MUI X shipped v8 majors.** So a changelog, ticket, or dependency note that says "MUI 8.x"
without naming a package is talking about MUI X (Data Grid, pickers, charts, tree view), a
mis-remembered version, or nothing at all — establish the package from the lockfile before acting
on it. The MUI X v8 peer range also *bounds* the Core major such an app can be on: every X v8
package accepts `@mui/material` at `^5.15.14 || ^6.0.0 || ^7.0.0` only — never v9 — and MUI's own
guidance is *"MUI X v8 only supports Material UI v7 by default"*
([MUI X v8 blog](https://mui.com/blog/mui-x-v8/)); v5/v6 additionally need bundler
`conditionNames`/`transpilePackages` config
([usage-with-material-ui-v5-v6](https://v8.mui.com/x/migration/usage-with-material-ui-v5-v6/)).

## Version & Compatibility

Registry facts, all verified 2026-08-08 from `registry.npmjs.org`:

| Line | Latest | Released |
|------|--------|----------|
| `@mui/material` v5 | 5.18.0 | 2025-07-08 |
| `@mui/material` v6 | 6.5.0 | 2025-07-08 |
| `@mui/material` v7 | 7.3.11 | 2026-05-07 |
| `@mui/material` v9 | 9.3.1 (`latest`) | 2026-08-06 |
| `@mui/x-data-grid` v8 | 8.29.2 | 2026-07-02 |
| `@mui/x-data-grid` v9 | 9.11.0 (`latest`) | 2026-08-06 |

Support matrix for **v7**, the major to write against by default
([v7 supported-platforms](https://v7.mui.com/material-ui/getting-started/supported-platforms/)):

| Requirement | v7 | v9, for contrast |
|-------------|-----|------------------|
| React | ≥ 17 (peer `^17 \|\| ^18 \|\| ^19`) | same |
| TypeScript | **≥ 4.9** (raised from 4.7 in v6) | ≥ 4.9 — *not restated in the v9 guide; assumed* |
| Node (SSR) | ≥ 14.0 | ≥ 14.0 |
| webpack | ≥ 5 | ≥ 5 |
| Browsers | Chrome 109, Firefox 115, Safari 15.4, Edge 121 | Chrome 117, Firefox 121, Safari 17, Edge 121 |

React 19.2.8 is `latest`; **React 20 does not exist** — if you were about to write "React 20", stop.

MUI X v8 is in **LTS**: security and regression fixes only, until **2028-04-08**
([MUI X support policy](https://mui.com/x/introduction/support/)). It is not abandoned, but it will
receive no features. The last 8.x drop was 8.29.2 on 2026-07-02.

### Verifying a claim against the right version — the docs trap

This is the highest-value operational rule in the module. **`mui.com` now serves v9 docs.** Checking
an API there and writing what you read is how you ship a v9-only prop into a v7 app.

| You want | Use | Do **not** use |
|----------|-----|----------------|
| Material UI v7 API | `https://v7.mui.com/material-ui/...` | `mui.com` (serves v9) |
| Material UI v6 / v5 API | `https://v6.mui.com/...`, `https://v5.mui.com/...` | — |
| MUI X v8 API | `https://v8.mui.com/x/...` | `mui.com/x/...` (serves X v9.11) |
| Any package's real API surface | `npm view <pkg>@<version>` / `unpkg.com/<pkg>@<version>/` | memory |

> **`v8.mui.com` is a booby trap.** It returns HTTP 200 and it is a real site — but it is the
> **MUI X v8** documentation snapshot, and its Material UI half serves **7.3.10**. There is no
> Material UI v8 behind it: `v8.mui.com/material-ui/migration/upgrade-to-v8/` is a 404 while
> `.../upgrade-to-v7/` is a 200. Worse,
> [mui.com/material-ui/getting-started/versions/](https://mui.com/material-ui/getting-started/versions/)
> lists a "v8" row linking there — it is the only released row with no release-notes link. An agent
> skimming that page will conclude Material UI v8 exists. It does not.

Migration guide URLs that actually resolve (probed 2026-08-08): `upgrade-to-v6/` 200,
`upgrade-to-v7/` 200, **`upgrade-to-v8/` 404**, `upgrade-to-v9/` 200 (and the v9 guide covers
**v7 → v9** in one hop). Every docs page also has an AI-readable Markdown twin — append `.md` to the
URL, e.g. `https://mui.com/material-ui/migration/upgrade-to-v9.md`. Prefer those when reading docs
programmatically.

## Breaking Changes By Major

What each major invalidated. Everything a model learned from v5-era code is in the left column.

| Change | v5 | v6 | v7 | v9 |
|--------|----|----|----|----|
| Grid layout API | `item` + `xs` | `Grid2` with `size` | `Grid` = new API; old ⇒ `GridLegacy` | `GridLegacy` **removed** |
| `components` / `componentsProps` | present | **deprecated** | still deprecated | **removed** |
| `makeStyles` / `withStyles` (JSS) | deprecated | last usable major | unusable | unusable |
| System props (`<Box mt={2}>`) | present | **deprecated** | deprecated | **removed** |
| CSS theme variables | `experimental_*` | stabilised + folded into `ThemeProvider` | recommended | only path |
| Deep imports (2+ levels) | worked | worked | **blocked** (`exports` field) | blocked |
| `Hidden`, `createMuiTheme` | present | present | **removed** | removed |
| Minimum TypeScript | 3.5 | 4.7 | **4.9** | 4.9 |

Two different patterns hide in that table, and conflating them is a mistake:

- **Deprecated in v6, removed in v9** — `components`/`componentsProps`, system props, JSS. On a
  v5/v6/v7 codebase these still *work*. The risk is stylistic (writing code the codebase is leaving)
  or the reverse: copying a v9 fix into a v7 app whose replacement API does not exist yet.
- **Removed outright at v6 or v7** — `Hidden`, `createMuiTheme`, `experimentalStyled`,
  `onBackdropClick`, `ListItem button`, deep imports, `Grid2`. These *are* hard breaks partway
  through the candidate range, which is why there is no single dialect valid across v5–v7.

## Packages & Imports

Package landscape as of 2026-08-08:

| Package | `latest` | Status |
|---------|----------|--------|
| `@mui/material`, `@mui/system`, `@mui/utils`, `@mui/icons-material` | 9.3.x | Active |
| `@mui/styled-engine` (Emotion), `@mui/styled-engine-sc` (styled-components) | 9.3.0 | Active |
| `@mui/lab` | 9.0.0-beta.8 | Perpetually beta — never stable |
| `@mui/base` | 5.0.0-beta.70 | **Deprecated** — npm: *"replaced by @base-ui/react"* |
| `@mui/styles` (JSS) | 6.4.8 | **Dead.** No stable v7+; every 7.x is a prerelease |
| `@mui/x-*` | 9.11.x | Versioned independently of Core — do **not** bump with Core |

**Never emit `@mui/styles`, `makeStyles`, `withStyles`, or `createStyles`.** The package is dead at
v7+ — no stable 7.x was ever published — its docs pages 301 away on v7+, and its own npm deprecation
notice points at the JSS migration guide. This is the single most likely v5-era reflex.

> **`npm i @mui/styles` will not stop you, and that is the trap.** The package declares no
> `@mui/material` peer at all — only `react` and `@types/react` (verified against the
> `@mui/styles@6.4.8` manifest) — so it installs cleanly beside a v7 or v9 Core and fails *silently*
> instead of loudly. It carries its own `@mui/private-theming` and `@mui/utils` at the 6.x line, so
> JSS styles resolve against a second, empty theming context rather than your app's `ThemeProvider`.
> The symptom is "my `theme.palette` values came out as defaults", not an install error. Do not treat
> a successful install as evidence that this rule is out of date.

**Deep imports break at v7.** v7 moved the package layout onto the Node `exports` field, and
two-level-plus paths (always private API) stopped resolving. The field is present in the v7 and v9
manifests and absent from the v5 and v6 ones — if you need to know whether a specific v7 patch has
it, check that version's `package.json` on unpkg rather than assuming.

```diff
- import createTheme from '@mui/material/styles/createTheme';   // 2+ levels — blocked at v7+
+ import { createTheme } from '@mui/material/styles';
```

One level is fine and is the *preferred* form:

```js
import Button from '@mui/material/Button';   // preferred — much faster dev rebuilds
import { Button } from '@mui/material';      // fine for prod bundle size, slow in dev
```

Note the rationale has **inverted** since the v4/v5 era: modern bundlers tree-shake barrel imports
correctly in production, so this is about *development* startup and rebuild speed, not bundle size
([minimizing-bundle-size](https://mui.com/material-ui/guides/minimizing-bundle-size/)). For
`@mui/icons-material`, named imports are up to six times slower in dev.

Also removed at v7: the `/modern` bundle aliases (`@mui/material/modern` and friends) and the Vite
`@mui/icons-material/esm/$1` alias. If you see them in a bundler config on a v7 codebase, they are
dead weight.

## Styling

**Emotion is the default styling engine — at v5, v6, v7 and v9 alike.** `@mui/material@9.3.1` still
declares `@emotion/react` and `@emotion/styled` as peers. Any claim that a recent major "removed the
Emotion dependency" is wrong; the shipped manifest refutes it.

The supported alternative is styled-components via the `@mui/styled-engine-sc` adapter (still
published, 9.3.0), which MUI discourages for server-rendered apps. Either way you import `styled`
from `@mui/material/styles`, not from the engine — so this changes your setup, not your component
code. Check the lockfile for `@emotion/*` vs `@mui/styled-engine-sc` to know which you are on.

Pigment CSS, introduced as opt-in in v6 and once slated to become the default, is **paused** —
[mui.com/blog/2026-and-beyond](https://mui.com/blog/2026-and-beyond/) reports the team put it on
hold, and the [pigment-css repo](https://github.com/mui/pigment-css) header reads *"Alpha phase,
currently, on hold."* `@pigment-css/react` is still sub-1.0. Do not propose it.

Write styles one of two ways:

```jsx
// sx — for one-off styling at the call site
<Box sx={{ mt: 2, color: 'primary.main', display: { xs: 'none', md: 'block' } }} />

// styled() — for reusable styled components
import { styled } from '@mui/material/styles';
const Item = styled('div')(({ theme }) => ({
  padding: theme.spacing(2),
  color: theme.palette.text.primary,
}));
```

> **`theme.vars` only exists when the theme was built with `cssVariables`.** Reading
> `theme.vars.palette.*` in an app that did not opt in is a runtime error, so use `theme.palette.*`
> until you have seen the `createTheme` call — then switch, because inside `styled`/`sx` on v7 the
> `theme.vars` form is the one that survives a mode change (see Theming).

## Theming

`ThemeProvider` + `createTheme` is the only recommended path. `CssVarsProvider` and `extendTheme`
are still exported at 9.3.1 but carry an `@deprecated` JSDoc tag in the shipped types.

```jsx
// ❌ v5/v6-era
import { experimental_extendTheme as extendTheme,
         Experimental_CssVarsProvider as CssVarsProvider } from '@mui/material/styles';
const theme = extendTheme();
<CssVarsProvider theme={theme}>…</CssVarsProvider>

// ✅ current (v6.0+, v7, v9)
import { ThemeProvider, createTheme } from '@mui/material/styles';
const theme = createTheme({
  cssVariables: { colorSchemeSelector: 'class' },
  colorSchemes: { light: true, dark: true },
});
<ThemeProvider theme={theme}>…</ThemeProvider>
```

Dark mode is `colorSchemes` + `useColorScheme()`, not `palette.mode`
([dark-mode](https://mui.com/material-ui/customization/dark-mode/)). `palette: { mode: 'dark' }`
works only with the default palette and has no system-preference detection or cross-tab sync.

Two mode-related traps that produce **no type error**:

1. **v6+: use `theme.applyStyles()`, not a `palette.mode` ternary.**
   ```diff
   const C = styled('button')(({ theme }) => ({
   -  borderColor: theme.palette.mode === 'dark' ? '#fff' : '#000',
   +  borderColor: '#000',
   +  ...theme.applyStyles('dark', { borderColor: '#fff' }),
   }));
   ```
2. **v7+: the theme object stops re-rendering on mode change.** With CSS variables and light/dark
   schemes, `useColorScheme().mode` updates but `theme.palette.mode` does not — so anything branching
   on it inside `styled`/`sx` silently freezes at the initial mode
   ([upgrade-to-v7](https://mui.com/material-ui/migration/upgrade-to-v7/)). Reference
   `theme.vars.palette.*` instead, or read `theme.colorSchemes.light/dark.palette.*` explicitly.
   `<ThemeProvider forceThemeRerender>` is the escape hatch.

Theme component `variants` moved inside `styleOverrides.root` in v6:

```diff
 createTheme({ components: { MuiButton: {
-  variants: [ … ],
+  styleOverrides: { root: { variants: [ … ] } },
 } } });
```

## Component API Deltas

### slots / slotProps

`slots`/`slotProps` is the surviving API. The **older** `components`/`componentsProps` family was
deprecated in v6, still worked through v7, and was removed in v9 — so on a v5–v7 codebase you will
*find* the old form and should still *write* the new one.

```jsx
// ❌ v5-era
<Tooltip title="Hi"
  components={{ Popper: CustomPopper }}
  componentsProps={{ popper: { placement: 'top' } }}
  TransitionComponent={Fade} TransitionProps={{ timeout: 500 }} />

// ✅ current
<Tooltip title="Hi"
  slots={{ popper: CustomPopper, transition: Fade }}
  slotProps={{ popper: { placement: 'top' }, transition: { timeout: 500 } }} />
```

Recurring mappings: `components`→`slots`, `componentsProps`→`slotProps`,
`TransitionComponent`/`TransitionProps`→`slots.transition`/`slotProps.transition`,
`PaperProps`→`slotProps.paper`, `imgProps`→`slotProps.img`, `ChipProps`→`slotProps.chip`,
`ListboxComponent`→`slots.listbox`, `MenuListProps`→`slotProps.list`. AvatarGroup's
`componentsProps.additionalAvatar` key becomes `slotProps.surplus`.

> **Do not derive the slot name by lower-casing.** Old `components` keys were PascalCase and slot keys
> are camelCase, so many pairs *look* like a pure casing flip (`Popper`→`popper`) — but several are
> genuine renames (`MenuListProps`→`list`, `additionalAvatar`→`surplus`, `inputProps`→`htmlInput`).
> For any component not in the list above, read that component's API page on the version-pinned docs
> site rather than guessing; each page has a Slots table.

### TextField — the casing trap

`InputProps` (capital I) and `inputProps` (lowercase) are **two different slots**:

```diff
 <TextField
-  InputProps={CustomInputProps}          // the MUI Input component
-  inputProps={CustomHtmlInputProps}      // the native <input> element
-  InputLabelProps={CustomLabelProps}
+  slotProps={{
+    input: CustomInputProps,
+    htmlInput: CustomHtmlInputProps,
+    inputLabel: CustomLabelProps,
+  }}
 />
```

### Removed outright

| Removed | In | Replacement |
|---------|----|-------------|
| `Hidden` | v7 | `sx={{ display: { xs: 'none', md: 'block' } }}` or `useMediaQuery` |
| `createMuiTheme` | v7 | `createTheme` |
| `experimentalStyled` | v7 | `styled` |
| `onBackdropClick` (Dialog/Modal) | v7 | `onClose(event, reason)`, check `reason === 'backdropClick'` |
| `ListItem` `button` / `selected` / `disabled` | v6 | `<ListItemButton />` |
| System props on Box/Stack/Typography/Grid/Link | v9 | `sx` |
| `Typography` `paragraph` | v9 | `sx={{ marginBottom: '16px' }}` |

Codemods exist for most of this: `deprecations/all` covers the slot renames and `v7.0.0/grid-props`
handles the Grid rename.

> **Do not run `@mui/codemod@latest` on a v5–v7 tree.** The same reasoning that makes `mui.com`
> dangerous applies to the codemod package: `latest` is the v9-era release, and its `deprecations/*`
> transforms rewrite code to APIs that may not exist in your major. Pin it — `npx @mui/codemod@7`
> — and review the diff rather than committing it wholesale.

## Grid & Layout

The single highest-churn API in the library. What actually ships, verified by probing
`unpkg.com/@mui/material@<v>/<Name>/index.js` (200 = ships, 404 = absent):

| Import path | v5 | v6 | v7 | v9 |
|-------------|----|----|----|----|
| `@mui/material/Grid` | legacy API | legacy API | **new API** | **new API** |
| `@mui/material/Grid2` | 404 | new API | **404** | 404 |
| `@mui/material/Unstable_Grid2` | new impl, **old props** | 404 | 404 | 404 |
| `@mui/material/GridLegacy` | 404 | 404 | legacy API | **404 — removed** |

> **`import { Grid2 } from '@mui/material'` is a hard break on v7+.** There is no `Grid2` export or
> folder — it was renamed to `Grid` in v7, and the old `Grid` became `GridLegacy`.

The API to write, on v7 and later:

```jsx
import Grid from '@mui/material/Grid';

// ❌ WRONG on v7+ — `item` and `xs` are not props of the modern Grid
<Grid container spacing={2}>
  <Grid item xs={12} sm={6} md={4}><Item /></Grid>
</Grid>

// ✅ RIGHT on v7+ (`sx` because a container is not full-width by default — see below)
<Grid container spacing={2} sx={{ width: '100%' }}>
  <Grid size={{ xs: 12, sm: 6, md: 4 }}><Item /></Grid>
</Grid>
```

> **On v5 or v6 the ❌ block is the correct code and the ✅ block does not compile** — `size` does not
> exist before v7. Confirm the major first; this is the single place where guessing costs you the
> "compiles on the first pass" criterion.

Exact prop translations: `xs={6}` → `size={6}` (bare value when uniform across breakpoints);
`xs` (bare boolean) → `size="grow"`; `xsOffset={2} smOffset={3}` → `offset={{ xs: 2, sm: 3 }}`;
`item` and `zeroMinWidth` are simply deleted.

Layout traps that produce no error:

- **A container is not full-width by default.** Add `sx={{ width: '100%' }}`, or `sx={{ flexGrow: 1 }}`
  when the parent is a flex container.
- **Spacing changed model.** v5 items included spacing inside their boxes; v6+ uses CSS `gap`. The
  migration guide itself warns this "may lead to unexpected changes to your app's layout."
- **`direction="column"` was always a misuse and is removed in v9.** Grid subdivides a layout into
  columns; use `Stack` for vertical stacking. On v5–v7 it is accepted and lays out wrong.
- **Theme keys and CSS classes move with the rename**: `MuiGrid2`→`MuiGrid`,
  `.MuiGrid2-root`→`.MuiGrid-root`, and on the legacy path `MuiGrid`→`MuiGridLegacy`. A
  `styleOverrides` block keyed on the old name silently stops applying.

## MUI X v8

An admin app's main surface is tables, so the Data Grid is where MUI X version drift most likely
bites. The v8 API lives at **`https://v8.mui.com/x/react-data-grid/`** — check it there, never on
`mui.com/x/`, which now serves v9.11.

**Which package, and therefore which features, is a licensing question first.** The npm `license`
field is the reliable discriminator:

| Tier | Packages | Gates |
|------|----------|-------|
| Community (MIT) | `@mui/x-data-grid`, `-date-pickers`, `-charts`, `-tree-view` | — |
| Pro (commercial) | `@mui/x-data-grid-pro`, `-date-pickers-pro`, `-charts-pro`, `-tree-view-pro` | Column/row pinning, tree data, master detail, row reordering, lazy loading; **all range pickers** (`DateRangePicker` and friends) |
| Premium (commercial) | `@mui/x-data-grid-premium`, `-charts-premium` | Row grouping, aggregation, Excel export, pivoting, cell/range selection, clipboard import |

Reaching for a Pro-gated feature on the Community package fails at import. Reaching for it *with* the
Pro package but no licence key does something worse: it renders, stays interactive, and paints a
watermark while logging via `console.error` — **in production as well as development**. Check for a
`LicenseInfo.setLicenseKey` call before assuming a tier.

v7 → v8 changes most likely to bite when editing existing grid code:

- **Row selection model changed shape** — from an array to `{ type: 'include' | 'exclude', ids: Set<GridRowId> }`. Code doing `selectionModel.map(...)` breaks.
- **`showToolbar` is now required** to render a toolbar; the `GridToolbar` slot approach is gone.
- **`LicenseInfo` moved** from `@mui/x-data-grid-pro` to `@mui/x-license`.
- **Theme key moved**: `theme.mixins.MuiDataGrid` → `theme.palette.DataGrid`.
- Formerly `unstable_`-prefixed features (`rowSpanning`, `listView`) lost the prefix.
- Slot renames: `paper`→`panelContent`, `pagination`→`basePagination`.
- TypeScript **5** is the minimum for the v8 line — higher than Core v7's 4.9.

Codemod: `npx @mui/x-codemod@latest v8.0.0/data-grid/preset-safe <path>`. Migration guides are
per-component-family: `https://v8.mui.com/x/migration/migration-data-grid-v7/` and the matching
`migration-pickers-v7` / `migration-charts-v7` / `migration-tree-view-v7`.

> **X versions move independently of Core.** Never bump `@mui/x-*` as part of a Core upgrade. X
> packages also pin each other *exactly* — `-pro@8.9.1` takes a regular (**not** peer) dependency on
> `x-data-grid@8.9.1` — so a mixed X version set does *not* fail resolution: npm quietly nests a
> second copy of the grid, and you end up with two module instances, a duplicated context and a
> licence check looking at the wrong one. Keep every `@mui/x-*` on a single version. And the
> `latest-v8` dist-tag is stale (8.29.1) while 8.29.2 is the newest 8.x; resolve `^8` rather than
> trusting the tag.

## TypeScript Integration

Custom theme keys — both interfaces are required:

```ts
declare module '@mui/material/styles' {
  interface Theme        { status: { danger: string } }
  interface ThemeOptions { status?: { danger?: string } }
}
```

Custom component variant, and a custom palette colour (which needs **two** augmentations):

```ts
declare module '@mui/material/Button' {
  interface ButtonPropsVariantOverrides { dashed: true }
  interface ButtonPropsColorOverrides   { custom: true }
}
declare module '@mui/material/styles' {
  interface Palette        { custom: Palette['primary'] }
  interface PaletteOptions { custom?: PaletteOptions['primary'] }
}
```

Typing `theme.vars` requires a side-effect import:
`import type {} from '@mui/material/themeCssVarsAugmentation';`

**v7 moved the augmentation target** — nested-path module declarations no longer resolve, and two
interfaces were renamed. This is a silent type break on upgrade:

```diff
-declare module '@mui/material/styles/createTypography' {
+declare module '@mui/material/styles' {
-  interface TypographyOptions { … }
+  interface TypographyVariantsOptions { … }
-  interface Typography { … }
+  interface TypographyVariants { … }
 }
```

Do not assign to `theme.vars` — it is autogenerated for CSS variables and throws.

**React ≤ 18 needs a `react-is` override.** MUI depends on `react-is@19`, which changed how React
elements are identified; a mismatch causes runtime errors in prop-type checks
([upgrade-to-v7](https://mui.com/material-ui/migration/upgrade-to-v7/)):

```json
{ "overrides": { "react-is": "^18.3.1" } }
```

## Gotchas

Behaviour changes that compile cleanly and fail at runtime or in tests:

- **Divider `orientation="vertical"` renders a `<div>`, not an `<hr>`** (v6). CSS or tests targeting
  `& hr` silently stop matching — use `` [`& .${dividerClasses.root}`] ``.
- **AccordionSummary is wrapped in an `<h3>`** (v6), and its root became a `<button>` with `<span>`
  content in 6.3.0. A nested `<Typography>` defaults to `<p>` and becomes invalid HTML inside it —
  pass `component="span"`.
- **Ripple timing broke click tests** (v6): `fireEvent.click(button)` may need
  `await act(async () => fireEvent.mouseDown(button))`. Affects Button, Checkbox, Chip, Radio,
  Switch, Tabs.
- **`Autocomplete.onInputChange` gained `"blur"`, `"selectOption"` and `"removeOption"` reasons**
  (v6), split out of `"reset"`. A `switch` on `reason` silently misses cases.
- **`InputLabel size="normal"` → `"medium"`** (v7); the `MuiInputLabel-sizeMedium` class is no longer
  emitted.
- **`SvgIcon` drops `data-testid` in production bundles** (v7) — never assert on it in a prod-mode E2E.
- **CSS class consolidation.** Composed classes became atomic pairs: `.MuiButton-textPrimary` →
  `.MuiButton-text.MuiButton-colorPrimary`, `.MuiAlert-standardSuccess` →
  `.MuiAlert-standard.MuiAlert-colorSuccess`. Deprecated in v6, removed in v9. Any selector or
  `styleOverrides` key using the composed form stops applying.
- **`styled(Box)` broke at v6** — `component` was removed from `BoxOwnProps`. Use `styled('div')` or
  cast `as typeof Box`.

MUI X gotchas — licensing, tiers and the v7→v8 deltas — are in **MUI X v8** above.

## References

| Source | Use |
|--------|-----|
| [registry.npmjs.org/@mui/material](https://registry.npmjs.org/@mui/material) | Proof no Core v8 exists; per-major release dates |
| [registry.npmjs.org/@mui/x-data-grid](https://registry.npmjs.org/@mui/x-data-grid) | Per-version manifests and their Core/React peer ranges |
| [Introducing Material UI and MUI X v9](https://mui.com/blog/introducing-mui-v9/) | Official "there is no Material UI v8" statement and the realignment rationale |
| [Introducing MUI X v8](https://mui.com/blog/mui-x-v8/) | "MUI X v8 only supports Material UI v7 by default" |
| [v7.mui.com](https://v7.mui.com/material-ui/getting-started/supported-platforms/) | v7 API and support matrix — the version-pinned site to check against |
| [v8.mui.com/x/](https://v8.mui.com/x/) | MUI X v8 API docs (**not** a Material UI v8 site) |
| [upgrade-to-v6](https://mui.com/material-ui/migration/upgrade-to-v6/) · [upgrade-to-v7](https://mui.com/material-ui/migration/upgrade-to-v7/) · [upgrade-to-v9](https://mui.com/material-ui/migration/upgrade-to-v9/) | Per-major breaking changes and codemods (there is no `upgrade-to-v8`) |
| [Upgrade to Grid v2](https://mui.com/material-ui/migration/upgrade-to-grid-v2/) | The exact Grid prop translations |
| [Migrating from deprecated APIs](https://mui.com/material-ui/migration/migrating-from-deprecated-apis/) | The `slots`/`slotProps` standardisation and its codemods |
| [MUI X licensing](https://mui.com/x/introduction/licensing/) · [MUI X support policy](https://mui.com/x/introduction/support/) | Pro/Premium gating, watermark behaviour, v8 LTS window |
| [minimizing-bundle-size](https://mui.com/material-ui/guides/minimizing-bundle-size/) | Current import guidance (dev speed, not bundle size) |
