---
description: Show every Sidekick command and what it needs
---

Call `sidekick_help` and show the user what is available.

One table, with a column for what each command NEEDS — the tool returns it per
command as `needs`: nothing, an account, or an account and a browser. Never leave
that column out and never infer it: a command listed without its requirement is
one somebody runs and gets refused by, and the ones that need nothing are the
reason they can be run before signing in.

Order it so those come first. Name
anything that writes outside `./help-center/` — the tool flags it as `writesTo`.

Keep it short: a table, not a tour.
