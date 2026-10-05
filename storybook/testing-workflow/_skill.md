---
name: Stories & CI Gates
path: /testing-workflow
tags: ["@engineering", "@dev", "@frontend", "@react", "@testing"]
learned: "2026-08-08"
last_verified: "2026-08-08"
confidence: 0.75
needs_reverification: false
notes: "Package facts (versions, publish dates, frozen packages, export moves) verified against registry.npmjs.org and the shipped @storybook/react 10.5.7 types on 2026-08-08. The ReactRenderer generic-parameter claim was checked against the shipped types, not the docs. Extracted into this community pack 2026-08-12."
---

# Storybook — Stories & CI Gates

## Overview

A story is the cheapest reviewable proof that a component change does what the ticket says — and
since Storybook 9 it can also be a real CI gate. This module covers current-major setup (the v9/v10
package moves that silently break v8-era config), CSF 3 stories, `play`-function interaction tests,
the Vitest addon gate, the a11y ratchet, and what snapshot and visual checks do and do not cover.

**Establish the repo's Storybook major before writing anything.** Everything below documents the
current major, and most of it is wrong for a 7.x or 8.x codebase — then open a neighbouring story
and copy its idiom, because the house convention beats the docs.

## Setup & Conventions

### Version reality

`storybook` majors and release dates, from the npm registry on 2026-08-08:

| Major | `.0.0` released | Note |
|-------|-----------------|------|
| 7.0.0 | 2023-03-31 | Still common in the wild — most idioms below differ on it |
| 8.0.0 | 2024-03-11 | |
| 9.0.0 | 2025-05-28 | The brutal one for story files |
| 10.0.0 | 2025-10-28 | Current; `latest` = **10.5.7** (2026-08-06) |

A repo's adoption date bounds the major it *started* on, not the major it runs today: routine
dependency bumps are exactly the kind of work a customer-facing changelog omits. From history alone
the honest position is *"the major current at adoption, current major unknown"* — settle it from
`package.json`, not from when Storybook first appeared.

> **Consequence, and it holds either way:** before writing a story, open an existing one and copy its
> idiom. Everything below documents the **current** major, and every example is wrong for 7.x. The
> mismatch is only partly loud — a removed package errors on import, but a removed *config key*
> (`docs.autodocs`, `globalTypes.defaultValue`) just silently stops working, and a renamed import that
> still resolves from a legacy package rots without complaint.

### `.storybook/main.ts` at the current major

```ts
import type { StorybookConfig } from '@storybook/react-vite';

const config: StorybookConfig = {
  framework: '@storybook/react-vite',   // '@storybook/react-webpack5' if the app is not on Vite
  stories: ['../src/**/*.mdx', '../src/**/*.stories.@(js|jsx|mjs|ts|tsx)'],
  addons: [
    '@storybook/addon-docs',
    '@storybook/addon-themes',   // the MUI theme decorator below
    '@storybook/addon-a11y',     // the a11y gate below
    '@storybook/addon-vitest',   // the CI gate below
  ],
  staticDirs: ['../public'],
};

export default config;
```

> **Every addon used later in this module must appear in that array.** `@storybook/addon-a11y` in
> particular fails open: set `parameters.a11y.test = 'error'` without registering the addon and the
> gate silently passes everything.

Two rules that catch people:

- **Import from the *framework*, not the renderer.** `@storybook/react-vite`, not `@storybook/react`.
  Storybook moved from renderer-based to framework-based config in v9. Types often still resolve from
  the renderer package, so this rots quietly rather than erroring.
- **v10 loads `main.ts` as ESM.** `require`, `__dirname` and `__filename` are undefined; a CJS
  `main.js` is unsupported; relative imports need file extensions; local addons need
  `import.meta.resolve('./my-addon.ts')`.

### Addons: most of what you would install no longer exists

Absorbed into core in v9 — **do not install or list these**: controls, actions, viewport,
interactions, backgrounds, measure, outline, toolbars, highlight. `@storybook/addon-essentials` was
removed entirely.

| v8-era import | Current |
|---------------|---------|
| `@storybook/test` | **`storybook/test`** (no scope) |
| `@storybook/addon-actions` | `storybook/actions` |
| `@storybook/blocks` | `@storybook/addon-docs/blocks` |
| `@storybook/preview-api`, `@storybook/theming` | `storybook/preview-api`, `storybook/theming` |
| `@storybook/experimental-addon-test` | `@storybook/addon-vitest` |

`@storybook/test`, `@storybook/addon-essentials` and `@storybook/addon-interactions` are all frozen at
**8.6.18** on npm — a file importing them is v8-era by definition. Still separately installed:
`@storybook/addon-docs`, `-a11y`, `-vitest`, `-themes`, `-links`.

### Theme decorator (MUI)

A component that reads the theme renders wrong — or throws — without a provider. Wire it globally
once:

```tsx
// .storybook/preview.tsx
import type { Preview, ReactRenderer } from '@storybook/react-vite';
import { withThemeFromJSXProvider } from '@storybook/addon-themes';
import { CssBaseline, ThemeProvider } from '@mui/material';
import { lightTheme, darkTheme } from '../src/themes';

const preview: Preview = {
  decorators: [
    withThemeFromJSXProvider<ReactRenderer>({
      themes: { light: lightTheme, dark: darkTheme },
      defaultTheme: 'light',
      Provider: ThemeProvider,
      GlobalStyles: CssBaseline,   // MUI's reset goes in the GlobalStyles slot
    }),
  ],
};

export default preview;
```

> **The generic parameter is `ReactRenderer`, not `Renderer`.** Verified against the shipped
> `@storybook/react@10.5.7` types: it exports `type ReactRenderer`, and `@storybook/react-vite` is
> `export * from '@storybook/react'` plus three of its own symbols. There is no bare `Renderer`
> export on either package, so `import type { Renderer } from '@storybook/react-vite'` is a TS2305
> "has no exported member" error. (Storybook's own addon-themes recipe writes
> `from '@storybook/your-renderer'`, which is a placeholder, not a package name.)

Without the addon, a hand-rolled decorator plus a toolbar global does the same job:

```tsx
const preview: Preview = {
  globalTypes: {
    theme: { toolbar: { title: 'Theme', icon: 'circlehollow',
                        items: ['light', 'dark'], dynamicTitle: true } },
  },
  initialGlobals: { theme: 'light' },       // NOT globalTypes.theme.defaultValue
  decorators: [(Story, context) => (
    <ThemeProvider theme={themes[context.globals.theme]}>
      <CssBaseline />
      <Story />
    </ThemeProvider>
  )],
};
```

> **`globals` → `initialGlobals` (v9), and `globalTypes[x].defaultValue` no longer works.** The
> symptom is a toolbar global that starts `undefined`, so the first render has no theme. No error.

> **Storybook's own MUI recipe is stale.** It advertises "Storybook >= 7.0" and still shows
> `addon-essentials`, the v6 `export const decorators` shape, and `argTypesRegex`. The
> `withThemeFromJSXProvider` API in it is current; the surrounding config is not.

## Writing Stories

CSF 3 is the format to write. (CSF Next / factories exists at v10 but is **preview** status and
forces a `preview.ts` import into every story file — not for a codebase that is three majors behind.)

```ts
import type { Meta, StoryObj } from '@storybook/react-vite';
import { Button } from './Button';

const meta = {
  component: Button,
} satisfies Meta<typeof Button>;

export default meta;
type Story = StoryObj<typeof meta>;

export const Primary: Story = { args: { primary: true } };
```

`satisfies Meta<typeof Button>` plus `StoryObj<typeof meta>` is what gives you arg autocompletion and
type errors on bad args. Args resolve preview → meta → story, most specific winning; compose with
`...Primary.args`.

### The story as a review artefact

For a component you changed, a story is the cheapest possible reviewable proof. Add one state per
branch you touched — the empty state, the error state, the long-string state (a localised UI
overflows layouts that look fine in English; German compound nouns like **Mitgliedsbescheinigung**
are the classic case — test with the longest real strings the product renders). Name stories after
the state, not the props.

### Interaction tests with `play`

```ts
import type { Meta, StoryObj } from '@storybook/react-vite';
import { fn, expect } from 'storybook/test';
import { LoginForm } from './LoginForm';

const meta = {
  component: LoginForm,
  args: { onSubmit: fn() },        // `fn()` makes it a spy you can assert on
} satisfies Meta<typeof LoginForm>;

export default meta;
type Story = StoryObj<typeof meta>;

export const FilledForm: Story = {
  play: async ({ args, canvas, userEvent }) => {
    await userEvent.type(canvas.getByLabelText('Email'), 'email@provider.com');
    await userEvent.type(canvas.getByLabelText('Password'), 'a-random-password');
    await userEvent.click(canvas.getByRole('button', { name: 'Log in' }));
    await expect(args.onSubmit).toHaveBeenCalled();
  },
};
```

- **`canvas` and `userEvent` come off the play context** at v10; `within(canvasElement)` still works
  but is the older idiom.
- **MUI renders `Dialog`, `Menu`, `Select` and `Tooltip` in a portal**, outside the story root — so
  `canvas.getByRole('dialog')` finds nothing. Use `screen` from `storybook/test`, which queries from
  `document`. This is the single most common MUI-plus-Storybook failure.
- **v8 removed implicit action args.** Storybook 7 auto-spied any `onX` prop; from v8 you must write
  `args: { onClick: fn() }` explicitly, or `expect(args.onClick).toHaveBeenCalled()` asserts against
  `undefined`. On a 7.x codebase the old behaviour still applies — another reason to read a
  neighbouring story first.

## As A Test Gate

**Whether Storybook is a gate or only an artefact is a repo fact, not a default.** Adoption does
not imply automation. Treat Storybook as a *review artefact* until you have seen a workflow file
that runs it.

If it is to become a gate, the current recommended path is the Vitest addon:

| Major | Gate |
|-------|------|
| 7–8 | `@storybook/test-runner` (Jest + Playwright) |
| 9 | `@storybook/addon-vitest` (renamed from `experimental-addon-test`) |
| **10** | **`@storybook/addon-vitest`** — test-runner is officially *"superseded"* |

`@storybook/test-runner` is not dead (0.24.4, 2026-05-14) and remains the **only** option for
non-Vite builders, since the Vitest addon works exclusively with Vite-based frameworks. If the app
is on the Webpack builder, that decides it.

```bash
npx storybook add @storybook/addon-vitest
npx playwright install chromium --with-deps
```

```ts
// vitest.config.ts  (Vitest 4)
import path from 'node:path';
import { fileURLToPath } from 'node:url';
import { defineConfig } from 'vitest/config';
import { storybookTest } from '@storybook/addon-vitest/vitest-plugin';
import { playwright } from '@vitest/browser-playwright';

const dirname = path.dirname(fileURLToPath(import.meta.url));

export default defineConfig({
  test: {
    projects: [{
      extends: true,
      plugins: [storybookTest({ configDir: path.join(dirname, '.storybook') })],
      test: {
        name: 'storybook',
        browser: {
          enabled: true, headless: true,
          provider: playwright({}),
          instances: [{ browser: 'chromium' }],
        },
      },
    }],
  },
});
```

Run with `vitest --project=storybook`. On Vitest 3 the provider is the string `'playwright'` and
versions below 3.2 use `test.workspace` instead of `test.projects`.

Accessibility is the same run. Install `@storybook/addon-a11y`; the single lever is
`parameters.a11y.test`:

```ts
const preview: Preview = {
  parameters: { a11y: { test: 'error' } },   // 'error' fails CI | 'todo' warns only | 'off'
};
```

`'todo'` is the ratchet: set `'error'` globally, then `'todo'` on the specific legacy components that
still violate, so new work is held to the bar without a big-bang cleanup. Settable at preview, meta
or story level.

> Let `npx storybook add` scaffold the config. The docs show a hand-written
> `.storybook/vitest.setup.ts` in `setupFiles`, but the shipped 10.5.7 CLI templates contain no
> `setupFiles` key — the plugin injects its own. Hand-writing it invites a mismatch.

### Snapshot and visual checks — a separate axis, and mostly not in this box

The `play` + a11y gate above proves a component *behaves* and is *reachable*. It will pass a
component whose layout is completely broken, which for a restyle or a long-string overflow is
the only thing you cared about. Do not report "the Storybook gate covers this" for a visual change.

| Path | Status |
|------|--------|
| `@storybook/addon-storyshots` | **Dead.** Frozen at 7.6.17 (2024-02-20), deprecated in favour of the test-runner, and *removed* in Storybook 8 — MIGRATION.md, "Storyshots has been removed": *"Snapshot testing has since fallen out of favor and is no longer recommended."* On a 7.x codebase you may find it wired up; do not carry it forward. |
| DOM snapshots via the Vitest addon | Works — a `play` function can `expect(canvas.getByRole(…)).toMatchSnapshot()` — but it asserts markup, not pixels, so it is noisy on refactors and blind to CSS. |
| Pixel diffing (`@chromatic-com/storybook` 5.3.0, `chromatic` 18.1.0) | The current real answer, and a hosted paid service. **Not something to adopt on a customer's repo inside a ticket** — propose it, do not wire it. |

For a frontend ticket on a repo without pixel diffing, the honest position is: stories are the
review artefact, a human looks at them, and pixel regression is out of scope until the repo's
owners say otherwise. Say that rather than implying automated visual coverage exists.

## References

| Source | Use |
|--------|-----|
| [registry.npmjs.org](https://registry.npmjs.org/) | Every `storybook` / `@storybook/*` version, publish date and peer range asserted here — re-resolve rather than trusting the numbers; they are true as of `last_verified` |
| [storybook.js.org/docs](https://storybook.js.org/docs) | Current-major docs — check the version selector before copying |
| [Storybook MIGRATION.md](https://github.com/storybookjs/storybook/blob/next/MIGRATION.md) | The per-major deltas; the authority for what silently breaks |
| [Interaction testing](https://storybook.js.org/docs/writing-tests/interaction-testing) · [Vitest addon](https://storybook.js.org/docs/writing-tests/integrations/vitest-addon) · [In CI](https://storybook.js.org/docs/writing-tests/in-ci) | `play`, the test gate, the CI workflow |
| [Accessibility testing](https://storybook.js.org/docs/writing-tests/accessibility-testing) | `parameters.a11y.test` and the `todo` ratchet |
| [addon-themes MUI guide](https://github.com/storybookjs/storybook/blob/next/code/addons/themes/docs/getting-started/material-ui.md) | `withThemeFromJSXProvider` for MUI (surrounding config is stale) |
