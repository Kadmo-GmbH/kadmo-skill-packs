---
name: Reddit
domain: reddit.com
match:
  - reddit.com
  - old.reddit.com
  - reddit_*
  - "@social"
tags: [social-media, community, marketing]
---

# Reddit

The Reddit knowledge pack teaches an agent how to participate in Reddit communities without triggering moderation, without looking like a bot, and without eroding the account's standing in any subreddit it touches. It covers feed reading, thread comprehension, comment drafting under the operator's persona, and the single write primitive that actually submits a comment. Everything else — inbox triage, karma monitoring, subreddit scans — is composed from those pieces.

Reddit is the strictest community platform in the public packs. Subreddit moderation is active, platform-level rate limiting is aggressive for young or low-karma accounts, and community voice matters more than it does on any other network. The defaults in this pack are deliberately conservative: every drafting workflow stops at the "ready to send" state so a human reviews and approves before anything is posted publicly.

## Browser Access

Every workflow in this pack drives `old.reddit.com` rather than the newer Reddit frontend. The old interface's DOM is dramatically more stable for extraction and interaction, and the selectors do not churn every few weeks the way the new interface's do. Assume that the connected browser is already logged in under the operator's account — authentication is out of scope for the pack itself.

| Tool | Usage |
|------|-------|
| `navigate` | Go to subreddit, thread, or user page |
| `snapshot` | Read thread content and comments |
| `click` | Vote buttons, reply links, sort options |
| `type` | Comment textarea, search |

## Workflows

The pack ships a small library of workflows that fall into three shapes: read primitives that snapshot a feed or thread, evaluation and drafting chains that produce decisions and draft comments under persona rules, and a single write primitive that submits a top-level comment. Compose them rather than editing them; the pack is designed for composition.

| Workflow | Description |
|----------|-------------|
| `reddit_get_feed` | Scan subreddit by sort (rising/new/hot/top) |
| `reddit_read_thread` | Extract post + all comments |
| `reddit_post_comment` | Submit top-level comment |
| `reddit_check_karma` | Account karma breakdown |
| `reddit_check_replies` | Inbox reply notifications |
| `reddit_scan_and_evaluate` | Scan feed + evaluate posts against strategy |

## Comments

A good Reddit comment adds something the thread did not already have — experience, data, a precise question, a correction. A comment that restates what other commenters said, or reads like a press release, will be downvoted into invisibility quickly.

- Match the subreddit's tone and norms
- Add value: share experience, provide data, ask good questions
- Don't self-promote unless the subreddit explicitly allows it
- Read the full thread before replying — don't repeat what's said

## Formatting

Reddit supports its own flavour of markdown, and using it well makes long comments readable instead of skimmed past. Break responses into paragraphs; quote the specific passage being responded to; use inline code for identifiers and code fences for anything longer than a line.

- Use markdown (Reddit supports it natively)
- Break long responses into paragraphs with headers
- Quote the specific part you're responding to

## Etiquette

Reddit has durable social norms beyond any single subreddit's rules. Breaking these norms is the fastest way to lose an account's credibility across many communities at once, even when no individual subreddit explicitly bans the behaviour.

- Don't edit comments to change meaning after replies
- Acknowledge when wrong — builds credibility
- Avoid sarcasm that could be misread

## Gotchas

These edge cases have all caused real incidents with automated posting before. They are small individually but expensive in aggregate, and keeping them resident in the pack's knowledge prevents rediscovering them the hard way.

- old.reddit.com selectors differ completely from new.reddit.com
- Rate limiting on comments: ~10 min cooldown for new/low-karma accounts
- Some subreddits require minimum karma to post
- Embed workflows use `old.reddit.com` — navigate there first
