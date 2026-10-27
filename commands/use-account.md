---
description: Switch to another Sidekick account you belong to
argument-hint: "[account-id-or-name]"
---

Call `sidekick_prompt` with `name: "use-account"` and follow what it
returns. Those are this command's instructions, kept on the server so they can
be improved without waiting for a plugin release.

The user's arguments, as typed after the command: $ARGUMENTS

If it answers `not_signed_in`, clear that yourself: call `sidekick_login`, which
opens a browser where the user signs in and chooses the account, then stop —
signing in already chose it. If it says the MCP is switched off, say that an
owner or admin can turn it back on under Account → MCP Server, and stop.
