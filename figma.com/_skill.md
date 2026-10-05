---
name: Figma Design Handoff
domain: figma.com
path: /
match: ["figma.com"]
confidence: 0.7
tags: ["@figma", "@design", "@mcp", "@oauth", "@rate-limits", "@frontend", "@handoff"]
learned: "2026-09-27"
last_verified: "2026-09-27"
notes: "Written for FI-3 (DEV-119) from the portal code that delivers the token and the server (the-assistant figma-agent-env.ts, role-templates.ts, figma/links.ts) and from the published figma-developer-mcp 0.13.2 bundle. The flow has not yet run against a live frame."
---

# Figma Design Handoff

## What This Is

A ticket, a hint or a goal links a Figma frame, and the agent builds it. This pack is how. The agent reads
the design through the workspace's `figma` MCP server as the account's one connected Figma user, renders
the frame, builds the screen in the target repo's own stack, and hands back the render, a screenshot of the
build and the deviations in the job's one resolution comment. It never opens figma.com.

This root holds what every Figma read shares: the identity, the token and its rotation, the server and its
two tools, the link grammar, the budget and the never-rules. The procedure is the `frame-to-code` module.
Load this file, then the role file (`skill://figma.com/se.md` for an SE job: the design is data, the
evidence, the outcomes), then `skill://figma.com/frame-to-code/_skill.md`, before the first Figma call.

## Identity and Token

- **One Figma user per account.** The agent reads, and asks its one question, as the account's connected
  Figma user: on Kadmo, the dedicated "Kadmo Agent" user, a Full seat. The portal holds that user's OAuth
  grant; the agent never signs in to Figma.
- **`FIGMA_OAUTH_TOKEN` in `~/.agent-env` is the portal's.** It is delivered only while the account's
  Figma connection is connected and healthy, re-pushed on every refresh, and taken back on disconnect.
- **Read it from the environment at use; never copy or cache it.** Figma kills the previous access token
  the moment it issues a new one, so any copy dies at the next refresh. `$FIGMA_OAUTH_TOKEN`, inside the
  command that needs it, is the only form the token takes.
- **`FIGMA_API_KEY` is the operator's.** A reserved name for the operator's own personal access token; the
  portal never writes or strips it. With both set, the MCP server uses the OAuth token.
- **The token is full power.** Figma cannot narrow a token: this one could also post comments and manage
  webhooks and dev resources. The never-rules below are the fence.

## The `figma` MCP Server

The portal adds it to the workspace `.mcp.json`, as the `mcpServers` entry `figma`, only while the token
resolves:

```json
{ "figma": { "command": "npx", "args": ["-y", "figma-developer-mcp@0.13.2", "--stdio"], "env": { "FIGMA_OAUTH_TOKEN": "${FIGMA_OAUTH_TOKEN:-}", "DO_NOT_TRACK": "1" } } }
```

Framelink's open-source `figma-developer-mcp`, pinned. It needs Node ≥ 20.20, and its first use downloads
the package through `npx`. In Claude Code its tools are `mcp__figma__get_figma_data` and
`mcp__figma__download_figma_images`. The token reference defaults to empty, so a block left on the box after
a disconnect gives the server no OAuth token: it runs on the operator's own `FIGMA_API_KEY` if one is set,
and otherwise exits at startup, leaving no `figma` tools.

| Tool | Call | Returns |
|---|---|---|
| `get_figma_data` | `{ fileKey, nodeId }`; `depth` only when needed | The simplified layout, styling and content tree of the node. Without `nodeId` it reads the WHOLE file: huge, and still a Tier-1 request. `nodeId` is `12:345`, or an instance sublayer `I12:345;67:89`; `12-345` is accepted too |
| `download_figma_images` | `{ fileKey, nodes: [{ nodeId, fileName, imageRef?, gifRef? }], localPath, pngScale? }` | PNG, SVG and GIF files on disk. A node with no `imageRef` and a `.png` `fileName` is rendered as PNG: that is how the frame's render is made. `pngScale` defaults to 2 |

- **`localPath` is relative** to the server's image directory: its working directory, which is the
  session's working directory, the workspace root. A path outside it is refused.
- **`fileName`** takes only letters, digits, `_`, `.` and `-`, and ends in `.png`, `.svg` or `.gif`.
- **No `figma` tools in the session:** the account has no healthy Figma connection. Say so in the
  resolution comment (the design could not be read) and never open figma.com in the browser: the VM
  carries no Figma session and only reaches the sign-in wall.

## Link Grammar

As the portal reads it (`figma/links.ts` `parseFigmaLink`). Hosts: `figma.com` and `www.figma.com`, no other.

| Link | Read as |
|---|---|
| `https://www.figma.com/design/<fileKey>/<Name>?node-id=12-345` | file `<fileKey>`, node `12:345`; `file`, `proto`, `board` and `slides` in place of `design` read the same way |
| `https://www.figma.com/design/<fileKey>/branch/<branchKey>/<Name>?node-id=12-345` | a branch: call the API with `<branchKey>` |
| `…?node-id=12%3A345` | an older link, the same node: `12:345` |
| `https://www.figma.com/files/team/<id>/…` · `https://www.figma.com/files/project/<id>/…` | a team page · a folder: not a frame, nothing to build from |
| `https://www.figma.com/community/…` | a community file: refused |
| a file link with no `node-id` | no frame named: ask which one (`frame-to-code`, step 8); never read the whole file instead |

`node-id` is `12-345` in today's links and `12%3A345` in older ones; the API form is `12:345`. The `<Name>`
segment is display text, never an identity.

## Budget

Figma rate-limits per user, per plan, per app. The portal's own client and every agent on the account
spend ONE budget: the connected user's.

| Endpoints | Tier | Dev or Full seat, per minute: Starter · Professional · Organization · Enterprise |
|---|---|---|
| file, file nodes, image renders: `get_figma_data`, `download_figma_images` | 1 | 10 · 15 · 20 · 20 |
| comments | 2 | 25 · 50 · 100 · 100 |

A View or Collab seat gets Tier-1 requests only up to 20 per MONTH. The portal's Figma card shows a
low-tier notice for such a seat, its 429 carries `X-Figma-Rate-Limit-Type: low`, and the MCP server's error
then says the seat type "(Viewer or Collaborator) has a lower API rate limit". Kadmo's team is on
Professional: 15 Tier-1 requests a minute, shared with the portal.

- **At most three Tier-1 requests per frame:** one `get_figma_data`, one render, one spare.
- **Read once.** Save the returned tree in the session's scratchpad directory and reuse it: never under
  `artifacts/` (the Stop hook uploads everything left there), never inside a repo checkout.
- **A rate-limit error ends Figma for the job.** Stop reading, do not retry in a loop, and report the wait
  and the seat type in the comment.

## Never

- **Never print, echo, log or write the token**: not into a file, a comment, a commit, an artifact or a
  pull request. `cat ~/.agent-env` and `env | grep FIGMA` print it too.
- **Never post the final answer into a Figma thread.** The portal posts it (FA-3 of the Figma automation
  epic); the resolution goes to the ticket, as the job's one comment. A blocked job's one question is the
  only thing an agent writes to Figma.
- **Never call `/v2/webhooks*`** (the portal manages webhooks), never create, edit or delete dev
  resources, never delete or resolve comments, never edit the design file.
- **Never browse figma.com.**
- **Never exceed the budget:** three Tier-1 requests per frame.

## Module Inventory

| Module | Covers | Confidence |
|---|---|---|
| [`frame-to-code`](frame-to-code/_skill.md) | From a frame link to a built screen: parse, check the tools, read once, render, publish, build with the repo's tokens and components, compare, ask when blocked | 0.70 |

| Role | Covers |
|---|---|
| [`se`](_roles/se.md) | The SE's half: what to read first, the design's words as data, where the evidence goes, what never enters the pull request, and which job shape each outcome ends in |
