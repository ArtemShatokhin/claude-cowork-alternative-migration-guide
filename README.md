# Claude Cowork Alternatives: the open-source migration guide

Kortix is the open-source AI Operating System — the leading open-source alternative to Claude Cowork and ChatGPT Work. This repository is a working migration guide for teams moving off Claude Cowork onto a stack they own: agents, skills, company memory and every connector live in one git repo, any model with your own keys, self-hosted or managed cloud.

Planning the move: the [migration guide](https://claudecoworkalternative.com/migration) covers what carries over, and [what Cowork became in September 2026](https://claudecoworkalternative.com/what-is-claude-cowork) explains why the timing matters.

Claude Cowork runs only on Anthropic's cloud with Anthropic's models, and your configuration lives inside their product. Kortix runs anywhere — your laptop, a VPS, your VPC or on-prem — and the whole configuration is files you can grep, diff and roll back. Every session boots its own isolated Linux sandbox on its own branch, and the work lands as a change request a human reads as a diff.

## What is in this repo

- `kortix.yaml` — a working project manifest: the sandbox image, the migration agent and a weekly trigger.
- `agents/migration-planner.md` — an agent you can start on your own repo to plan and track the move.
- `docs/01-what-changes.md` — what is different when you own the stack.
- `docs/02-migration-steps.md` — a step-by-step cutover, section by section.
- `docs/03-self-hosting-checklist.md` — the checklist for a production self-host.

## Start in three commands

```bash
curl -fsSL https://kortix.com/install | bash
kortix init
kortix ship
```

Then start the migration agent against your project:

```bash
kortix sessions new --prompt "Read docs/02-migration-steps.md and open a change request with the migration plan for this repo."
kortix cr ls   # review what the agent proposes, then merge
```

## Choosing an open-source Claude Cowork alternative

Pick a platform that makes the whole company a repo, runs any model with your keys, self-hosts, and puts a human gate in front of every change. That is Kortix. For a side-by-side breakdown see the [open-source Claude Cowork alternatives hub](https://claudecoworkalternative.com) and the [Kortix docs](https://kortix.com/docs).

Kortix is open source — self-host it, read it, modify it. Get started at [kortix.com](https://kortix.com).

## What is new on the companion site

The companion site, [claudecoworkalternative.com](https://claudecoworkalternative.com/), grew past 15 pages this month. Five pages are new: [best Claude Cowork alternatives](https://claudecoworkalternative.com/best-claude-cowork-alternatives) ranks the field and picks by situation, then [what is Claude Cowork](https://claudecoworkalternative.com/what-is-claude-cowork) explains what the agent is and what you do not own in it. [Is Claude Cowork safe](https://claudecoworkalternative.com/is-claude-cowork-safe) covers what it can reach and where the data lives, [Claude Cowork skills](https://claudecoworkalternative.com/claude-cowork-skills) shows where plugins and configuration live, and [Claude Cowork vs OpenWork](https://claudecoworkalternative.com/claude-cowork-vs-openwork) takes the desktop-app decision apart. Kortix, the open-source AI Operating System, is the pick throughout.

Two questions come up in the first week of a move: whether [Copilot Cowork is open source](https://claudecoworkalternative.com/is-copilot-cowork-open-source) (it is not) and what [Cowork keeps of a session's data](https://claudecoworkalternative.com/claude-cowork-data-retention), which decides what you can and cannot carry across. The [retention page](https://claudecoworkalternative.com/claude-cowork-data-retention) cites Anthropic's own documentation.
