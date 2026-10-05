---
name: Frame to Code Implementation
domain: figma.com
parent: figma.com
path: "/design/<fileKey>"
confidence: 0.7
tags: ["@figma", "@frontend", "@design-tokens", "@components", "@evidence", "@publish-artifact", "@blocked"]
learned: "2026-09-27"
last_verified: "2026-09-27"
notes: "Tool behaviour read from the published figma-developer-mcp 0.13.2 bundle, publish_artifact and screenshot from the SideButton tool schemas, the link grammar from the portal parser. Not yet drilled against a live frame."
---

# Frame to Code Implementation

## What This Is

The procedure for a job whose ticket, hint or goal links a Figma frame: read the frame once through the
`figma` MCP server, render it, build it in the target repo with the repo's own tokens and components, run
the build, and hand back the render, a screenshot and the deviations. The root
(`skill://figma.com/_skill.md`) holds the identity, the tools, the link grammar, the budget and the
never-rules; every step here assumes them.

The job's prompt decides the shape of its one resolution comment (DONE with its pull request, a handoff,
BLOCKED). This module decides what that comment says about the design: the render's link, the build
screenshot's link and the deviations, or the one line saying the design could not be read, and why.

## Procedure

1. **Parse the link** (root, *Link Grammar*): `fileKey`, the branch key on a branch link, and `nodeId` in
   API form, `12:345`. No frame to read (no `node-id`, a team page, a folder, a community file) → ask
   which frame (step 8).
2. **Check the tools.** No `mcp__figma__get_figma_data` in the session → the design cannot be read. Say so
   in the comment and stop reading Figma; no browser. Work built from the ticket's text alone is reported
   as that, never as matching the frame.
3. **Read once.** `get_figma_data({ fileKey, nodeId })`, no `depth`. Write the returned tree to a file in
   the session's scratchpad directory and work from that file for the rest of the job.
4. **Render.** One `download_figma_images` call:
   `{ fileKey, nodes: [{ nodeId, fileName: "<frame>-render.png" }], localPath: ".figma/<fileKey>" }`, with
   the assets the tree shows the build will need in the same call (step 6). The files land in
   `.figma/<fileKey>/` under the workspace root. Never commit that folder.
5. **Publish the render.** `publish_artifact` with its absolute path
   (`<workspace root>/.figma/<fileKey>/<frame>-render.png`: under the home directory, 25 MB at most). Cite
   the returned `download_url` in the resolution comment. Without the tool, the ops skill's
   `artifacts/` fallback applies.
6. **Build it in the workspace's own stack.** Find the target repo (the one the ticket names, else the one
   that owns the screen) and read its design system, tokens and existing components FIRST: a tokens file
   or package, the theme, `tailwind.config.*`, CSS custom properties, `components/`, stories. Map each
   Figma style to an existing token and each element to an existing component instead of hard-coding
   values; follow the repo's conventions. The assets the frame needs come out of the render call's budget:
   an image fill by its `imageRef`, an icon by its `nodeId` with an `.svg` name.
7. **Compare.** Run the app the way the repo runs it (its dev server), screenshot the built screen at the
   frame's size (*Tips*) to a file with the `screenshot` tool's `path`
   (`<workspace root>/.figma/<fileKey>/<frame>-build.png`), and publish it with `publish_artifact` beside
   the render. List the deviations in the comment (layout, spacing, type, colour, missing assets or
   states), or say none were found.
8. **Blocked** (an ambiguous frame, a missing state, a conflicting spec) → ONE question by REST (*Asking in
   Figma*), then end the job in its BLOCKED shape.
9. **Throughout:** the root's never-rules and the three-request budget.

## Asking in Figma

One question, one request: `POST https://api.figma.com/v1/files/<fileKey>/comments` (Tier 2) with
`Authorization: Bearer $FIGMA_OAUTH_TOKEN` and `Content-Type: application/json`.

| The job's hint | Body |
|---|---|
| names the Figma thread's root comment | `{ "message": "<the question>", "comment_id": "<root comment id>" }`: a reply names the ROOT comment, never another reply |
| names no thread | `{ "message": "<the question>", "client_meta": { "node_id": "<nodeId>", "node_offset": { "x": 0, "y": 0 } } }`: pinned to the frame |

```bash
Q="$(mktemp -d)/question.json"      # scratch: never artifacts/, never a checkout
jq -n --arg m "$QUESTION" --arg c "$ROOT_COMMENT_ID" '{message: $m, comment_id: $c}' > "$Q"
# no thread named: pin it to the frame instead
# jq -n --arg m "$QUESTION" --arg n "$NODE_ID" '{message: $m, client_meta: {node_id: $n, node_offset: {x: 0, y: 0}}}' > "$Q"
curl -sS -X POST "https://api.figma.com/v1/files/$FILE_KEY/comments" \
  -H "Authorization: Bearer $FIGMA_OAUTH_TOKEN" \
  -H "Content-Type: application/json" \
  --data-binary @"$Q"
```

The shell expands the token inside the `curl` call, so it never appears in the command text, the output
or a file. No `echo`, no `set -x`, no `curl -v`. With neither a thread nor a node to pin to (a file link
with no `node-id`, a team page, a folder), the question goes into the BLOCKED comment only.

Then the BLOCKED shape of the job's prompt: what blocks, what would unblock it, and the question as asked.
Never a second question; never the final answer in Figma.

## Data Model

`get_figma_data` returns 0.13.2's default `tree` text, not JSON:

| Block | Holds |
|---|---|
| `NAME:` | the file's name |
| `GLOBAL_VARS:` | the style table (YAML): layouts, fills, strokes, effects and text styles, each under an id |
| `ELEMENTS:` | repeated node templates, which a node line names with `template=` |
| `COMPONENTS:` · `COMPONENT_SETS:` | the components and component sets the node uses |
| `NODES:` | one line per node, indented by depth: `[TYPE] "name" #<nodeId>`, then `layout=`, `fills=`, `strokes=`, `effects=`, `opacity=`, `borderRadius=`, `textStyle=`, `componentId=`, `componentProperties=`, `text="…"` |

A style value on a node line is inline JSON or an id into `GLOBAL_VARS`. An image fill carries the
`imageRef` that `download_figma_images` needs. `componentId=` names the Figma component an instance comes
from: the first place to look for the repo's counterpart.

## States

| State | Signal | Do |
|---|---|---|
| Ready | `mcp__figma__get_figma_data` and `mcp__figma__download_figma_images` in the session | the procedure |
| No connection | no `figma` entry under `mcpServers` in the workspace `.mcp.json`, no tools | say the design could not be read; no browser |
| Server down | a `figma` entry, but no tools | the same, naming the server: it needs Node ≥ 20.20 and fetches its package through `npx` on first use |
| Rate-limited | a 429; the error text may carry `Retry after <n> seconds` | stop reading Figma, no retry; report the wait |
| Low seat | the error says the seat type "(Viewer or Collaborator) has a lower API rate limit"; the 429 carries `X-Figma-Rate-Limit-Type: low` | stop reading Figma; report the seat type: 20 Tier-1 requests a month |
| Token refused | an authentication error mid-job | the token rotated or the connection went away: stop reading Figma and report it; never search for another token |
| Blocked | an ambiguous frame, a missing state, a conflicting spec | one question (*Asking in Figma*), then BLOCKED |

## Tips

- **Name files after the frame.** `<frame>` is the frame's name reduced to letters, digits, `_`, `.` and
  `-` (`Checkout / Summary` → `checkout-summary`). Keep verdict words (`pass`, `fail`, `merged`,
  `blocked`, `conflict`) out of evidence file names: the gate string-matches the comment.
- **Compare like with like.** The render is at `pngScale` 2, twice the frame's size in pixels. Screenshot
  the build at the frame's width and height (`region` crops the `screenshot` tool's viewport; a repo's own
  screenshot script can set any size). When the browser cannot take the frame's width, say which width was
  compared.
- **Tokens by name first, value second.** A Figma style whose name matches a repo token maps to that token
  even when the values differ; list the difference. A value with no token goes into the deviations,
  however it was built.
- **`componentId=` before markup.** An instance's Figma component names the repo component to reuse
  before anything new is written.
- **One render call.** Put the frame's render and its assets in the same `download_figma_images` call.
  Each format is its own render request (PNG, SVG) and image fills take one more lookup, so a call that
  mixes them spends the spare.

## Gotchas

- **Figma's render URLs expire.** The tool downloads each file; publish that file. Never cite a Figma
  image URL in a comment: it stops working.
- **A `.env` in the server's working directory overrides the environment.** figma-developer-mcp loads
  `.env` from its cwd with override, so a Figma key in a workspace `.env` replaces the portal's token.
  Never put Figma keys in a workspace `.env`.
- **A branch link needs the branch key.** The main file's key reads the main file, not the branch the
  designer linked.
- **A whole-file read is the budget trap.** `get_figma_data` without `nodeId` returns the entire file and
  still costs a Tier-1 request: on a View or Collab seat, one of 20 for the month.
- **Relative paths go to `~/workspace`.** `publish_artifact` and `screenshot` resolve a relative `path`
  against `~/workspace`, not the session's working directory; pass absolute paths.
- **25 MB per artifact.** A large frame at scale 2 can exceed it; the spare request re-renders it at
  `pngScale: 1`.
