---
description: Sign in to your Sidekick account (in a browser)
---

Sign the user in so the workspace, publish and widget commands work.

Call `sidekick_login`. It opens the Sidekick dashboard in a browser and the user
signs in themselves — email, Google, SSO or a magic link, whatever their account
uses. **Do not ask for a password.** No credential passes through you; the tool
reads the session the web app creates, shows a "you can close this window"
confirmation, closes the browser itself, and stores only the token in
`~/.sidekick/credentials.json`.

**Tell the user what to do while it runs, and what happened after.** Before the
tool returns, say a browser is opening and they should sign in there. The tool
blocks until they do (or for a few minutes), so this is the one command where
the session looks idle on purpose — say so, so silence does not read as a hang.
When it returns, report the outcome:

- `signed_in` — start with the result's `message` as it is ("Signed in to
  Acme"). Someone in more than one account chooses which in the browser before
  the tool returns; say before it opens that the browser may ask. They can
  switch later with `/sidekickhero:use-account`. The account's DEFAULT workspace is selected automatically and
  returned as `defaultWorkspace` / `savingFlowsTo`. Tell the user plainly which
  workspace their flows will be saved to, and that they can change it with
  `/sidekickhero:use-workspace`. Do not make them choose — the default is already set.
  **If the result carries an `onboarding` block, do what it says BEFORE showing
  the commands.** It appears only for an account OWNER who has never chosen a
  focus level, and it asks one question: how much of each screen should be
  covered by default. Put it in your own words, explain all three levels —
  `original`, `highlight` (the default), `focus` — and say plainly that they can
  leave it as it is and change it later with `/sidekickhero:focus-level`. Set the
  level they pick with `sidekick_focus_level`. If they would rather not decide,
  **set nothing**: recordings then follow the workspace's own setting, and a
  level set here would override that on this machine from now on. One question —
  do not turn it into an interview.

  **Then call `sidekick_help` and show the commands**, grouped by what each needs,
  so a newly signed-in user sees what they can do next — leading with
  `/sidekickhero:explore`. One short list, not a tour.
- `window_closed` — the browser was closed before sign-in finished; offer to run
  it again and to leave the window open until it confirms.
- `timed_out` — offer to run it again, or with a longer `waitSeconds`.

If the machine has no display and cannot open a browser, and only then, fall
back to `sidekick_login_password`: ask for the email, then the password in a
separate message, and never repeat it back.
