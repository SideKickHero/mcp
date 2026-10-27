---
description: Show or set whether problem reports are sent to the Sidekick team
argument-hint: "[on|off]"
---

Show or change whether **problem reports** are sent to the Sidekick team when a
Sidekick tool goes wrong (a step it could not measure, a save it could not
finish, a tool that failed).

Call `sidekick_problem_reports`. With no argument it reports the current setting.
When the user passed `$1`, map `on` → `enabled: true` and `off` → `enabled: false`.

It is off until the user agrees. A report holds the tool, what went wrong and its
technical details, and the page's address without its query string — never
screenshots or anything typed into forms. Turning it on also sends the reports
held so far this session. `SIDEKICK_MCP_TELEMETRY=0` (or `1`) in the server's
environment overrides this setting; if the tool says so, tell the user.
