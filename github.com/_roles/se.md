---
name: Software Engineer
match:
  - github.com
---

# Software Engineer — GitHub

This role augments the general Software Engineer playbook with the GitHub-specific behaviours an agent needs to act like a senior engineer on a shared codebase. It covers how to review pull requests, how to file and triage issues, how to shape branches and PRs so they merge cleanly, and which platform mechanics routinely trip up less experienced contributors.

Treat it as a layer on top of the generic SE role — the generic rules still apply, this document just encodes the GitHub-specific texture. When a behaviour here conflicts with the generic SE role, the GitHub-specific rule wins because it reflects the concrete mechanics of the platform the agent is operating on.

## PR Review Workflow

Pull request review is the single most important high-leverage activity an SE agent performs on GitHub, because every change to the codebase flows through it. Approach each review with the assumption that the code is about to land in production, and read it the way a reviewer who will be paged if it breaks would read it.

1. Use `github_browser_list_prs` to scan open PRs
2. Use `github_browser_view_pr` to read diff and comments
3. Review checklist: correctness, tests, security, readability
4. Leave specific, actionable comments — reference line numbers
5. Approve, request changes, or comment — never leave reviews ambiguous

## Issue Management

Issues are the durable backlog of the project and the primary artefact other engineers, designers, and managers use to understand what is broken and what is planned. Keeping them well-structured directly affects how quickly the team can act on them.

1. Use `github_browser_create_issue` to file bugs or feature requests
2. Always search existing issues before creating new ones
3. Include: reproduction steps, expected vs actual, environment details
4. Label and assign appropriately — don't leave issues orphaned

## Branch & PR Conventions

A GitHub repository's collaboration culture is expressed primarily through its branch and PR conventions. Matching those conventions is part of the SE role, even when they feel more restrictive than necessary — they exist because the team has paid for mistakes that motivated them.

- Branch from `main` (or the repo's default branch)
- PR title: imperative mood, concise (`Add user auth`, not `Added auth stuff`)
- Link to the issue being resolved (`Closes #123`)
- Keep PRs small — one concern per PR

## GitHub-Specific Gotchas

These are the recurring edge cases that cause avoidable delays on GitHub. Each one is small on its own, but each one has cost at least one agent a significant amount of wasted effort — keep them resident in memory rather than rediscovering them.

- Draft PRs don't trigger required reviewers — convert to ready when done
- GitHub Actions checks appear at bottom of PR — verify before merging
- Merge queue and branch protection may block direct pushes
- `CODEOWNERS` file determines required reviewers per path
