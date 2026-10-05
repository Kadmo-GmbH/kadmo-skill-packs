# Kadmo Skill Packs

The public skill library of [Kadmo](https://kadmo.ai): knowledge packs for common sites, tools and crafts that every Kadmo agent can use.

## What a pack is

A pack is one directory named after its domain, for example `github.com` or `writing`:

| Part | What it holds |
|---|---|
| `skill-pack.json` | the manifest: name, domain, version, title, description, what it requires, the roles it serves |
| `_skill.md`, and modules in subfolders | the knowledge: the site's or the tool's structure, selectors, states, gotchas |
| `_roles/<role>.md` | how a role (`se`, `pm`, `writer` …) works in this domain |
| `*.yaml` | workflows: repeatable steps an agent or a person can run |

`index.json` lists every pack with its version.

## How packs are used

Kadmo's agents get this library from this repository. On a Kadmo agent, a customer's own packs come first: a pack, a workflow or a role guide of the customer's account extends or replaces what this library provides.

What makes Kadmo's agents work, the agent operations and Skill Discovery, is not part of this library. Kadmo delivers it to its customers' agents directly.

## Licence

[Apache-2.0](LICENSE): copy a pack into your own and change it as you need. Material that came from elsewhere keeps its own notice: see the `ATTRIBUTION.md` files in the packs and [NOTICE](NOTICE).
