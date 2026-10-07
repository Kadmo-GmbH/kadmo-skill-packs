* **This repository is public**: the Kadmo skill library under Apache-2.0, read by anyone and installed on every Kadmo agent; write every file as public text.
* **Source of truth**: README.md (what a pack is, how packs are used) and NAMING.md (titles, lengths, uniqueness).
* **Never here**: the agent ops (`agents`), Skill Discovery (`_roles/sd.md`, `_souls/sd.md`, `agent_sd_*`), a customer's name, an internal playbook, a price or outreach text; check every change for them by hand before a merge.
* **A pack**: one directory named after its domain with `skill-pack.json`, `_skill.md`, its modules, `_roles/<role>.md` and `*.yaml` workflows; bump its `version` and update `index.json` in the same change.
* **Precedence on an agent**: a customer's own pack, workflow or role guide extends or replaces what this library provides; write packs so that an override stays possible.
* **Ticket creation needs approval**: propose the ticket first (title, type, Problem, Fix) and create it only after the user's yes; an unattended agent creates one only when its task requires it.
* **Conflicts**: when code, the source-of-truth documents and instructions disagree, stop and ask which wins before changing anything.
* **Unclear task**: if requirements are unclear, conflicting or ambiguous in a way that changes the work, stop and ask before starting; never build on an assumption.
* **Complete work**: deliver complete, working features; no placeholders, mocks or "later" inside the scope.
* **Out of scope**: do not build adjacent work; leave a TODO comment at the spot and propose a ticket for it.
* **Blockers**: investigate the root cause of anything that blocks progress and propose a ticket for it.
* **Verify in every context**: test each change directly where it runs (locally, on test, on production) before calling it done; the same change can pass in one and fail in another.
* **Displays are not proof**: never trust a status indicator, KPI or data display; they can be broken or mocked.
* **Code only**: open pull requests; never merge, tag, release or deploy, and change nothing under `.github/`.
* **Workflows deliver only**: a workflow builds and delivers; no tests, checks or scans in it and never a `pull_request` trigger; checks run by hand before a pull request.
* **Kadmo only**: no old brand names, legacy paths, redirects or compatibility defaults; dead code is deleted, not ported.
* **Secrets**: never print, log or commit a secret or an environment value; name the variable only.
* **No ticket assumed**: a task may belong to any Jira project or to no ticket at all; never require or invent one.
* **New requirements**: when the user clarifies or adds one, record it in the source-of-truth documents and propose the matching ticket changes.
* **Docs explain WHAT and WHY**: *.md files cover architecture, specifications and business logic with tables, JSON schemas and Mermaid diagrams, not implementation syntax.
* **Docs carry no code**: never implementation code (JS/TS/Java/Python/Rust/Go) in *.md; allowed blocks are JSON, Mermaid, bash/CLI, YAML/TOML/.env and SQL.
