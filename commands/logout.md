---
description: Sign out of Sidekick on this machine
---

Call the `sidekick_logout` tool. It deletes `~/.sidekick/credentials.json`.

Say plainly what remains afterwards: `/sidekickhero:help` and
`/sidekickhero:problem-reports` keep working, because they never needed an
account. Any saved browser sessions under a project's `.sidekick/` are
untouched — those belong to the application being documented, not to the
Sidekick account, and are removed by deleting that folder.
