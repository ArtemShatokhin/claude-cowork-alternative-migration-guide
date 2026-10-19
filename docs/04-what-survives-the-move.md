# What survives when you migrate off Claude Cowork to open source

Most of a Claude Cowork setup does not leave Anthropic's account. What survives is what you can download: instruction text, skill source, memory text, task transcripts and finished files. Connector grants and scheduled task definitions are account state, rebuilt on the other side. Kortix is the open-source AI Operating System, the leading open-source alternative to Claude Cowork and ChatGPT Work, with your agents, their skills, your company memory, and every connector in one git repo you own.

Export first and you finish with an inventory and a repo. Rebuild first and you spend a month rediscovering which instruction mattered.

## Carry-over happens inside Claude

Anthropic's [September 16, 2026 release note](https://support.claude.com/en/articles/12138966-release-notes) says what happens to existing work: "Your chats, Cowork tasks, projects, connectors, and skills carry over." The [help article](https://support.claude.com/en/articles/16761823-claude-cowork-and-chat-are-one-claude) adds: "Once your account has the new experience, you can't switch back to separate 'Chat' and 'Cowork' options."

That is an inventory of the setup, not a portable bundle. Anthropic's [export page](https://support.claude.com/en/articles/9450526-export-your-claude-data) limits the archive to "conversation data and the user data for your account," and says it can't be imported into another Claude account. Every step here is manual: nothing imports a Claude account.

## The export-first checklist

1. Request the account export; the link expires after 24 hours.
2. Copy the memory text and the global instructions.
3. Collect the ZIP for each custom skill.
4. Download task data and outputs.
5. Record connectors, folder grants and schedules, then commit the inventory.

Once the account closes, no page holds connector names or schedules.

## The inventory: what Anthropic documents, and where it lands

| Artifact | What Anthropic documents | Where it lands in your repo |
| --- | --- | --- |
| Instructions and skills | Custom skills export before a move; instructions not exportable. | `agents/*.md`, `skills/<name>/SKILL.md`, and a `kortix.yaml` grant. |
| Connector grants | Not exportable; an account move revokes the authorizations. | Connector names in `kortix.yaml`; reconnect each app. |
| Memory | Copy the memory text, or ask Claude to write it verbatim. | A memory file in the repo, reviewed as a change. |
| Scheduled tasks | Pause, resume, delete and run on demand; not exportable. | A `triggers:` entry with a cron and prompt. |
| Files and outputs | Per-task download, or "Export your Cowork data" for everything. | Committed outputs; uploaded files exported separately. |
| Account history | Individual export covers chats and account data. | An archive beside the repo; reference, not configuration. |

### A data archive is not configuration

The archive holds conversations and account records, not a manifest another product can read. Keep it as evidence.

### Memory exports cleanly

Anthropic's [memory export](https://support.claude.com/en/articles/12123587-import-and-export-your-memory-from-claude) says you can "bring it to another AI service by copying and pasting it into a local file on your computer." Put that text in a repo memory file, reviewed like any change.

### Skills are files you already have

Anthropic says there is "no migration path for skills." Its [skills page](https://support.claude.com/en/articles/12512180-use-skills-in-claude) describes uploading a ZIP and no download control, so the export is the folder you packaged. Keep the source ZIP beside `SKILL.md`.

### Connector grants are authorizations, not records

Anthropic's [account-move page](https://support.claude.com/en/articles/9267400-move-your-personal-claude-account-to-a-team-or-enterprise-organization) says all authorizations are revoked and each app is reconnected. A grant is a token tied to a person's identity, so nothing downloads.

## The cutover sequence that does not stall

### Export first

Request the archive, copy the memory text, and download each task you still need. If the link expires, request a new one.

### Map each artifact to a file

Each table row needs a destination before you write an agent. [`kortix.yaml`](../kortix.yaml) is the manifest and [`agents/migration-planner.md`](../agents/migration-planner.md) plans the move. Its first step is to read [`docs/02-migration-steps.md`](02-migration-steps.md) and inventory the prompts, tools and schedules Cowork runs today. That inventory is the spec.

### Run the planner against your repo

```bash
curl -fsSL https://kortix.com/install | bash
kortix init
kortix ship
```

Then start the agent:

```bash
kortix sessions new --prompt "Read docs/02-migration-steps.md and open a change request with the migration plan for this repo."
kortix cr ls   # review what the agent proposes, then merge
```

The agent opens one change request that adds agents and skills as markdown files, with an owner and an Allow, Ask or Block rule per tool. Session work reaches `main` through a change request. Merge is default-deny for agents, so a person reads the plan as a diff.

### Review the diff, then run one real task

The migration is the first session that produces the same output as the Cowork task it replaces. Run both stacks for one cycle, compare, then retire the Cowork task. [`docs/03-self-hosting-checklist.md`](03-self-hosting-checklist.md) covers the production checks.

### The manifest

```yaml
version: 2
project: claude-cowork-migration
image: kortix/base:latest

agents:
  migration-planner:
    file: agents/migration-planner.md
    connectors: all

triggers:
  - slug: weekly-migration-review
    type: cron
    cron: "0 9 * * 1"
    prompt: Review the open migration tasks in this repo, update the plan, and open a change request.
```

The `triggers:` block is where schedules land. The weekly entry shows the shape every ported schedule takes.

## Scheduled tasks still running in Claude

Anthropic documents scheduled tasks as remote sessions that run on their cadence with the desktop app closed. Published controls are pause, resume, delete and run on demand. Anthropic documents no export of the definition, so read the prompt and cadence from the [Scheduled page](https://support.claude.com/en/articles/13854387-schedule-recurring-tasks-in-claude-cowork) and rewrite them as a trigger.

Port one, run it once in a change request, compare, then pause the Claude task. Deleting first removes the only description of what it did. A task that needs local files runs only where they are, and a cloud session reaches connected folders only while the desktop app is open.

## FAQ

### How to transfer Claude Cowork to a new computer?

Sign in to the same Claude account on the new computer. [Cloud sessions and files follow the account](https://support.claude.com/en/articles/15520349-use-claude-cowork-on-web-desktop-and-mobile), so work from the old machine is available. Reconnect folders and local connectors, which are tied to the machine. Locally stored desktop sessions do not move; copy anything you want to keep.

### Can I disable Claude Cowork?

Team and Enterprise Owners can turn Cowork off in [Organization settings > Cowork](https://support.claude.com/en/articles/13455879-use-cowork-on-team-and-enterprise-plans). On an individual Pro or Max account with the merged experience, there is no separate mode to disable; it is part of every conversation. You can turn memory off for one task.

### How to switch from Claude to Claude Cowork?

After the September 16, 2026 merge, you don't switch. Describe the task in any conversation and Claude decides whether it needs a quick answer or a multi-step run. Before the rollout, accounts still show Chat and Cowork options; there is nothing to enable. Once moved, you can't switch back.

### Does Claude Cowork keep working after closing?

Cloud tasks keep running after you close the laptop or desktop app, and scheduled tasks run with no device online. Tasks that need local files, a connector, the browser or computer use need the desktop app open on that machine. If it closes, the session keeps running without them.

## Start with the export, not the rebuild

Anthropic gives you an account archive, a memory export, task downloads and a [Cowork data export](https://support.claude.com/en/articles/15520349-use-claude-cowork-on-web-desktop-and-mobile), and no portable configuration. A migration plan closes that gap one artifact at a time, in files a reviewer can read.

Kortix is built for that ownership: run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free. Start with the [migration guide](https://claudecoworkalternative.com/migration), see how the skill layer maps in [Claude Cowork skills](https://claudecoworkalternative.com/claude-cowork-skills), or read [what Claude Cowork became](https://claudecoworkalternative.com/what-is-claude-cowork). Commands are in the [Kortix docs](https://kortix.com/docs); code is on [Kortix on GitHub](https://github.com/kortix-ai/suna).
