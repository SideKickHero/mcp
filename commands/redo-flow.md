---
description: Record a saved flow again, replacing its steps in place
argument-hint: "\"<flow title or id>\"  [--url <url>] [--hint \"<what changed>\"] [--window <width>x<height>] [--mobile [device]] [--user-agent \"<ua>\"] [--safety read-only|safe-forms|full-auto] [--style <pack>] [--voice-over [voice]] [--no-memory]"
---

This drives a browser: a window opens and the user signs in themselves.
It needs Playwright — `npm install playwright && npx playwright install chromium`.

Call `sidekick_prompt` with `name: "redo-flow"` and follow what it
returns. Those are this command's instructions, kept on the server so they can
be improved without waiting for a plugin release.

The user's arguments, as typed after the command: $ARGUMENTS

If it answers `not_signed_in`, clear that yourself: ask for the email, call
`sidekick_login`, ask for the password separately — never repeat it back — then
call `sidekick_prompt` again. If it says the MCP is switched off, say that an owner
or admin can turn it back on under Account → MCP Server, and stop.
