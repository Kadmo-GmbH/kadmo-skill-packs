---
name: Slack
domain: slack.com
match:
  - slack.com
  - app.slack.com
  - slack_*
tags: [communication, team, messaging]
---

# Slack

Team messaging, channels, and thread management. Agents drive the `app.slack.com` web client. Slack desktop apps share most selectors with the web client but expose additional native chrome that agents do not need.

## Browser Access

Requires the user to be signed in to at least one workspace in the connected browser. Cookies persist across sessions. If the workspace switcher is open or the user belongs to multiple workspaces, navigation must always include the workspace ID in the URL: `app.slack.com/client/<TEAM_ID>/<CHANNEL_ID>`.

| Tool | Usage |
|------|-------|
| `navigate` | Go to channel, thread, or DM URL |
| `snapshot` | Read messages, channel state, sidebar |
| `click` | Channels, threads, reactions, action buttons |
| `type` | Message compose, search, slash commands |

## Workspace anatomy

| Region | Contains |
|---|---|
| Top bar | Workspace name, search, history nav |
| Left rail | Workspace icons (multi-workspace), notifications |
| Channel sidebar | Sections, channels (`#…`), DMs, group DMs, apps |
| Main view | Channel or DM message stream |
| Right pane | Thread, channel details, file viewer, search results — overlays main |

Channels are listed in customizable sections (Starred, Channels, Direct Messages, Apps, plus user-defined). Unread channels show in bold; channels with mentions show a badge.

## URL patterns

| URL | Surface |
|---|---|
| `/client/<team>/<channel>` | Channel |
| `/client/<team>/<dm>` | Direct message conversation |
| `/archives/<channel>/p<ts>` | Permalink to a single message |
| `/archives/<channel>/p<ts>?thread_ts=<parent>` | Permalink to a thread reply |
| `/messages/<channel>` | Legacy channel URL — Slack auto-redirects |

Message permalinks are the most reliable way for agents to anchor a conversation context. Always copy the permalink rather than scraping rendered timestamps.

## Threads

Threads open in the right pane. Replies posted to the channel via the compose box stay in-channel; replies posted in the right-pane compose box stay in the thread. The "Also send to #channel" checkbox bridges them — leave it off unless the thread genuinely needs to surface in the channel.

- Reply in threads, not in the channel — keep channels scannable.
- Threads with >10 messages: summarize before acting.
- When creating tickets from Slack, always link the ticket back to the message permalink.

## Search

Slack search supports modifiers that scope dramatically:

| Modifier | Effect |
|---|---|
| `from:@user` | Authored by user |
| `to:@user` | DMs to user |
| `in:#channel` | Restrict to channel |
| `before:YYYY-MM-DD` / `after:` / `on:` | Date range |
| `has:link` / `has:pin` / `has:reaction` | Filter by attachment type |
| `is:thread` / `is:saved` / `is:starred` | Special collections |

Modifiers stack and can be entered as free text in the search bar. Always quote multi-word phrases.

## Messaging conventions

- Use `@mentions` sparingly — only for direct action items.
- Keep messages concise — one topic per message.
- Use emoji reactions (`:eyes:`, `:thumbsup:`, `:white_check_mark:`) to acknowledge without adding noise.

## Formatting

- Bullet points for lists and action items.
- Bold key decisions or outcomes (`*bold*` in the markdown subset; toolbar applies `<b>` automatically).
- Code blocks (triple backtick) for technical content, snippets, and stack traces.
- Inline code (single backtick) for filenames, identifiers, and short commands.

## Status, DND, and notifications

The user status (avatar overlay) and Do Not Disturb schedule live in the account menu. Bots that post during DND deliver, but mobile push is suppressed. Always check the recipient's status emoji before paging — `:vacation:` and `:no_bell:` typically mean delay non-urgent asks.

## Gotchas

- Slack web app uses heavy React virtualization — selectors need ARIA names or `data-qa` attributes, not raw class names.
- Message compose uses a `contenteditable` div, not a `<textarea>` — `browser.type` works, `browser.fill` does not.
- The thread sidebar overlays the channel — clicks intended for the channel can hit thread elements when both are open. Close the thread first.
- Workspace switching changes the entire DOM and loses the prior message draft.
- Slash commands (`/remind`, `/giphy`) execute on Enter, not Send — confirm with a snapshot before assuming the message posted as text.
- Channel names are case-insensitive and `-` separated; the URL slug uses the channel ID, not the name, so renames don't break permalinks.
- Edited messages show an `(edited)` marker but no diff; if audit history matters, prefer pulling from Slack's export rather than scraping the UI.
