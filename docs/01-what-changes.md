# What changes when you own the stack

Claude Cowork is a closed platform: Anthropic's cloud, Anthropic's models, your configuration inside their product. Kortix is the open-source AI Management System, and moving to it changes four things that matter.

**The company becomes a git repo.** Agents, skills, memory, connector config and triggers are files. You can grep the whole company, diff any change and roll any part of it back. There is no export step, because there was never a black box.

**Any model, your keys.** Pick the model per agent, per session or per message — Claude, OpenAI, Gemini or your own OpenAI-compatible endpoint. Switch the day a better model lands instead of waiting for a vendor.

**It runs where you decide.** Self-host on a laptop to try it, then a VPS, your VPC or on-prem — or use managed cloud. No self-host is the difference between owning the system and renting it.

**A human gate on every change.** Each session runs in its own isolated Linux sandbox on its own branch. Work reaches main only through a change request you read as a diff. Merge is default-deny for agents.

The rest of this repo turns those four claims into a cutover plan you can run.
