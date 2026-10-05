---
name: GitHub
domain: github.com
match:
  - github.com
  - github_*
tags: [devops, code-review, issues]
---

# GitHub

Source code hosting, pull requests, issues, code review, and CI/CD via GitHub Actions. Agents drive the public web UI at `github.com`. GitHub Enterprise Cloud (`*.ghe.com`) shares the same DOM. GitHub Enterprise Server self-hosted instances usually match too but may lag a release behind.

## Browser Access

Requires the user to be signed in to GitHub in the connected browser. SSO and SAML sessions persist as long as cookies are kept. Personal access tokens are not used by browser flows — those are for the REST/GraphQL API and out of scope here.

| Tool | Usage |
|------|-------|
| `navigate` | Go to repo, PR, issue, code, or actions URL |
| `snapshot` | Read page structure, file lists, diff content |
| `click` | Tabs, buttons, links, review comments |
| `type` | Search, comment composers, branch picker |

## Repository surfaces

A repository at `github.com/<owner>/<repo>` exposes a fixed tab bar:

| Tab | URL suffix | Purpose |
|---|---|---|
| Code | `/` | File tree, README, branch picker |
| Issues | `/issues` | Issue list, filter bar |
| Pull requests | `/pulls` | PR list, filter bar |
| Actions | `/actions` | Workflow runs |
| Projects | `/projects` | Linked Projects boards |
| Wiki | `/wiki` | Markdown wiki (often empty) |
| Security | `/security` | Advisories, Dependabot |
| Insights | `/pulse` | Traffic, contributors, network |
| Settings | `/settings` | Repo configuration (admin only) |

The Code tab is the entry point for everything else. Branch and tag selection happens via the branch picker dropdown; the URL embeds the branch as `tree/<branch>/<path>` or `blob/<branch>/<path>`.

## Pull requests

A PR lives at `/<owner>/<repo>/pull/<num>` with sub-tabs:

| Sub-tab | URL | Purpose |
|---|---|---|
| Conversation | `/pull/<n>` | Title, description, comments, reviews, status checks |
| Commits | `/pull/<n>/commits` | Commit list |
| Checks | `/pull/<n>/checks` | CI run output |
| Files changed | `/pull/<n>/files` | Diff viewer with inline review |

Comment scopes:

- **PR-level comment** — bottom of the Conversation tab, applies to whole PR.
- **Review comment** — file-level, anchored to a line range in the diff.
- **Review summary** — Approve / Request changes / Comment, plus optional summary text.
- **Suggestion** — special review comment that proposes a code change the author can apply with one click.

PR drafts hide the merge button and skip required reviewers until promoted to "Ready for review." Required status checks appear at the bottom of the Conversation tab.

## Issues

`/<owner>/<repo>/issues` lists all open issues with the standard filter bar. Each issue at `/<owner>/<repo>/issues/<num>` supports comments, labels, milestones, assignees, projects, linked PRs, and a sidebar of metadata. Closing reasons are `completed`, `not planned`, or `duplicate` — the reason determines downstream automation behavior.

## Code search

The site search at `/<owner>/<repo>/search` supports the modern code-search syntax:

| Modifier | Effect |
|---|---|
| `path:<glob>` | Restrict to files matching glob |
| `language:<lang>` | Restrict by detected language |
| `repo:<owner/repo>` | Cross-repo search |
| `org:<org>` | All repos in org |
| `symbol:<name>` | Match defined symbols (functions, classes) |
| `extension:<ext>` | Restrict by file extension |
| `"phrase"` | Exact phrase |
| `/regex/` | Regex match |

The classic search is still available at `/search?q=…` but modern code search returns more relevant results for code-shaped queries.

## Diff viewer

Files changed (`/pull/<n>/files`) renders a unified or split diff. Each hunk has a `+` button at the line gutter that opens an inline review comment. Multi-line review comments require a click-and-drag across the line numbers in the diff. Review comments are queued as a "pending review" until the user submits with Approve / Request changes / Comment.

## Conventions

- Keep PRs focused — one logical change per PR.
- Imperative PR titles ("Add feature", not "Added feature").
- Description should answer: what changed, why, how to test.
- Reproduce bugs before filing — include steps, expected vs actual.
- Approve if the approach is sound, even if you'd do it differently.
- Block only for correctness, security, or breaking changes.

## Common patterns

**View a PR:** navigate to `/<owner>/<repo>/pull/<n>`, snapshot for status, click `Files changed` for diff.

**List PRs:** navigate to `/<owner>/<repo>/pulls`, snapshot list. Use the filter bar (`is:open author:@me`, `is:draft`, `review-requested:@me`) to narrow.

**Create issue:** use the `github_browser_create_issue` workflow or navigate to `/<owner>/<repo>/issues/new`.

**Read CI failures:** navigate to `/<owner>/<repo>/pull/<n>/checks`, click into the failed job, snapshot the log pane (live-tails recent lines).

## Gotchas

- Check runs are AJAX-loaded — wait for the status badge to settle from "in progress" before grabbing the result.
- Required reviewers may be governed by CODEOWNERS — the merge button stays disabled until the owning team approves.
- Force-pushes invalidate prior review comments (they appear under "Outdated"); always re-review after a force-push.
- The branch picker has both a "branches" and "tags" tab inside the dropdown — check the active tab before choosing a ref.
- Some orgs enforce SSO that periodically re-prompts — surfacing as a redirect to `github.com/orgs/<org>/sso`. Detect and pause the workflow rather than retry.
