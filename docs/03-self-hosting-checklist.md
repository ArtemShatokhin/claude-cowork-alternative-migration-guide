# Open-source self-hosting checklist

Kortix self-hosts free, and it is open source. Run this checklist before you call a self-hosted deployment production.

- [ ] `kortix self-host start` runs the full stack on one box, or you have deployed it into your VPC.
- [ ] The project repo is backed by your own remote and every change goes through a change request.
- [ ] Each agent is a markdown file; each skill is a file; memory is files. Nothing critical lives only in a UI.
- [ ] Tool permissions are set per call — Allow, Ask or Block — with Ask on send, pay and delete.
- [ ] Connector credentials are brokered server-side and are not present in any file or sandbox image.
- [ ] Secrets are encrypted at rest with a per-project key.
- [ ] Roles and groups are defined; SAML SSO and SCIM are configured if the team needs them.
- [ ] The audit trail is captured and reviewed.
- [ ] A backup of the git repo exists outside the host.

When every box is ticked, the system is yours end to end: the agents, the data, the skills, the connectors, the memory and the configuration.
