---
description: Sign in to your Sidekick account (in a browser)
---

Sign the user in so the workspace, publish and widget commands work.

Call `sidekick_login`. It opens the Sidekick dashboard in the user's own
default browser, where they sign in however their account works — email,
Google, SSO or a magic link — or are already signed in, and click **Authorize**.
**Do not ask for a password.** No credential passes through you: the browser
hands a one-time code back to the tool on this machine, which trades it for a
token and stores only that in `~/.sidekick/credentials.json`.

**Tell the user what to do while it runs, and what happened after.** Before the
tool returns, say a browser tab is opening and they should approve the sign-in
there (and sign in first if asked). If nothing opens, the tool sends the link
as a progress message — pass it on. The tool
blocks until they do (or for a few minutes), so this is the one command where
the session looks idle on purpose — say so, so silence does not read as a hang.
When it returns, report the outcome:

- `signed_in` — start with the result's `message` as it is ("Signed in to
  Acme"). Someone in more than one account chooses which on the Authorize page
  before the tool returns; say before it opens that the page may ask. They can
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
- `denied` — they clicked Cancel on the Authorize page; offer to run it again.
- `failed` — the sign-in code could not be used (usually it expired); offer to
  run it again.
- `window_closed` — only on an older Sidekick server, which still signs in
  through a separate window: it was closed before sign-in finished; offer to run
  it again and to leave the window open until it confirms. Google sign-in does
  not work in that window — suggest email, SSO or a magic link instead.
- `timed_out` — offer to run it again, or with a longer `waitSeconds`.

If the machine has no display and cannot open a browser, and only then, fall
back to `sidekick_login_password`: ask for the email, then the password in a
separate message, and never repeat it back.
