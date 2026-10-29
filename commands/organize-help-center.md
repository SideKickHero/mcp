---
description: Organize the help center's flows into categories with AI
argument-hint: "[--force] [--apply] [context…]"
---

Call `sidekick_prompt` with `name: "organize-help-center"` and follow what it
returns. Those are this command's instructions, kept on the server so they can
be improved without waiting for a plugin release.

The user's arguments, as typed after the command: $ARGUMENTS

If it answers `not_signed_in`, clear that yourself: ask for the email, call
`sidekick_login`, ask for the password separately — never repeat it back — then
call `sidekick_prompt` again. If it says the MCP is switched off, say that an owner
or admin can turn it back on under Account → MCP Server, and stop.
