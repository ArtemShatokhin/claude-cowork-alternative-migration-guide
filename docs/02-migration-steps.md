# Open-source migration steps

Moving from Claude Cowork to open-source Kortix — the open-source AI Management System — happens in six steps. Work through them in order, and end each with an agent run on a real task rather than a checklist tick.

## 1. Stand up Kortix

```bash
curl -fsSL https://kortix.com/install | bash
kortix init
kortix ship
```

You now have a project repo with `kortix.yaml`, an agent and a trigger. Confirm a session starts and opens a change request.

## 2. Inventory the jobs

List what runs in Claude Cowork today: the recurring prompts, the tools each one touches, who owns it. Put the list in this repo so it is reviewable.

## 3. Map each job to an agent

One job, one agent markdown file. Add the skills it needs and wire only the connectors it uses. Set Allow, Ask or Block per tool call — put Ask on anything that sends, pays or deletes.

## 4. Move scheduled work to triggers

A cron or a signed webhook starts a session with nobody present. Port each scheduled Claude Cowork task to a trigger in `kortix.yaml` and run it once in a change request.

## 5. Cut over

Run both stacks in parallel for one cycle. Compare the output of each agent against the job it replaced. Merge the Kortix agent when it matches, then retire the Claude Cowork task.

## 6. Keep the work

Every session commits back into the repo. Let the memory files accumulate, and review change requests weekly so the company improves one reviewed change at a time.
