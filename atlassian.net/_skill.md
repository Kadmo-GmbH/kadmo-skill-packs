---
name: Jira
domain: atlassian.net
match:
  - atlassian.net
  - jira_*
  - "@ops"
tags: [project-management, issues, sprints]
---

# Jira

Issue tracking, sprint planning, and project management on Atlassian Cloud. Agents drive the `<workspace>.atlassian.net` web UI. Jira Server / Data Center self-hosted instances diverge enough in DOM that selectors here are not guaranteed to work there.

## Browser Access

Requires the user to be signed in to Jira/Atlassian in the connected browser. SSO sessions persist across runs. Set `JIRA_BROWSER_URL` in environment variables to the workspace base URL (`https://acme.atlassian.net`) so workflows do not have to hardcode it.

| Tool | Usage |
|------|-------|
| `navigate` | Go to board, issue, search, or filter URL |
| `snapshot` | Read board state, issue panel, search results |
| `click` | Buttons, links, issue cards, transition options |
| `type` | Forms, search fields, JQL input |

## Project types

Jira projects come in two flavors that share the issue model but expose different UI:

| Type | URL | Notes |
|---|---|---|
| Company-managed (classic) | `/jira/projects/<KEY>` | Shared schemes, JQL, custom workflows |
| Team-managed (next-gen) | `/jira/software/projects/<KEY>` | Per-project config, simpler workflow |

Both publish the same `/browse/<KEY>-<num>` URL for individual issues, so issue navigation works the same regardless of project type.

## Boards, backlog, and sprint

A Software project exposes:

| Surface | URL suffix | Purpose |
|---|---|---|
| Board | `/boards/<id>` | Active sprint or kanban columns |
| Backlog | `/backlog` | Refinement queue, sprint planning |
| Reports | `/reports` | Burndown, velocity |
| Issues | `/issues` | Project-scoped issue search |
| Pages | `/pages` | Linked Confluence pages |

Drag-and-drop on the board moves cards between columns and triggers the workflow transition associated with the destination column. Drag actions on the backlog reorder by rank.

## Issues

An issue at `/browse/<KEY>-<num>` exposes:

- Summary (title, editable inline)
- Description (Atlassian Document Format — rendered, editable inline)
- Status pill (click to transition)
- Sidebar: Assignee, Reporter, Priority, Labels, Components, Fix versions, Story points, Sprint, Linked issues, Attachments, Web links
- Activity feed: Comments, History, Work log

Issue keys are stable across renames and project moves — they are the canonical identifier.

## JQL

JQL (Jira Query Language) powers all advanced search. Filter examples:

| JQL | Result |
|---|---|
| `project = SCRUM AND status = "In Progress"` | All in-progress in SCRUM |
| `assignee = currentUser() AND resolution = Unresolved` | My open work |
| `sprint in openSprints() AND project = SCRUM` | Active sprint contents |
| `created >= -7d AND project = SCRUM` | New this week |
| `labels = "needs-review"` | Tagged for review |
| `parent = SCRUM-123` | Subtasks of an epic/story |
| `text ~ "search phrase"` | Full-text search |

JQL is entered at `/issues/?jql=…` (URL-encode the query). Saved filters live at `/issues/?filter=<id>`.

## Transitions

Workflow transitions are surfaced in two places:

1. The **status pill** at the top of an issue — clicking opens the available transitions for the current status.
2. The board column drop target — releasing a dragged card runs the column's transition.

Available transitions depend on the workflow scheme attached to the project; they are not universal. Always read the current status and available transitions from the issue page before scripting a move.

## Linking

Issues link via:

- **Hierarchy**: Epic ↔ Story / Task ↔ Subtask. Epic link sits in the sidebar.
- **Issue links**: typed (`blocks`, `is blocked by`, `relates to`, `duplicates`, `is duplicated by`, `causes`, `is caused by`). Visible in the Linked Issues section.
- **Web links**: free-form URL + title. Useful for Slack permalinks, GitHub PR URLs, Figma frames.

## Components, labels, versions

Three orthogonal taxonomies:

- **Components** — pre-defined, project-scoped, often map to teams or services.
- **Labels** — free-form, global, lowercase-hyphenated by convention (`needs-review`, `tech-debt`, `customer-impact`).
- **Fix versions** — release identifiers, project-scoped.

Use components when an admin should curate the taxonomy. Use labels when anyone should add new tags ad hoc.

## Conventions

- Always include: summary, description, acceptance criteria.
- Past tense in bug descriptions ("Button failed to load").
- Link to source context (Slack thread, page URL, log line).
- Assign to the right team — don't leave unassigned.
- Don't reopen resolved tickets — create a new one and link with `relates to` / `caused by`.

## Priority

| Level | When to use |
|---|---|
| P1 / Highest | Production outage, data loss |
| P2 / High | Broken feature affecting users |
| P3 / Medium | Enhancement or improvement |
| P4 / Low | Cosmetic or nice-to-have |

## Common patterns

**View issue:** navigate to `/browse/<KEY>-<num>`, snapshot for full state.

**Search:** navigate to `/issues/?jql=<encoded>`, snapshot results table.

**Transition:** click the status pill, choose target transition, fill required fields if the workflow demands them.

**Create issue:** use the `jira_browser_create_issue` workflow or click Create in the top bar — fill summary, project, type, then submit.

## Gotchas

- The issue page is heavily SPA — fields auto-save on blur; scripting must wait for the save toast before moving on.
- Sidebar fields like Sprint and Epic only appear on Software projects; Service / Business projects expose different fields.
- JQL is case-insensitive on field names but case-sensitive on string values.
- The board view caches sprint contents — after a transition, refresh before reading the new column state.
- `/browse/<KEY>` (without number) redirects to the project; double-check that the issue key is fully qualified.
- API-style endpoints (`/rest/api/3/…`) are not part of this browser pack — use the Jira REST API directly when JSON output matters.
