# Sidekick Hero — help-center guides from your coding agent

Write and record your help center from inside Claude Code, Codex, Cursor or
VS Code. The agent reads your codebase for the real routes and on-screen
labels, records each flow in a browser window you can watch, and saves the
result as step-by-step guides in your Sidekick Hero workspace — ready to publish
as a help center or an in-app agent.

Install it in your coding agent first (see [Setup](#setup) below). Then, in the
project you want documented:

```
/sidekickhero:login
/sidekickhero:explore http://localhost:3000
```

## Setup

### Claude Code

```sh
claude plugin marketplace add SideKickHero/mcp
claude plugin install sidekickhero@sidekickhero
```

### Other editors

Point your editor at the npm package. `@latest` makes it start the newest
release each time the editor launches it.

**Cursor** — `.cursor/mcp.json` in the project, or `~/.cursor/mcp.json` for
every project:

```json
{
  "mcpServers": {
    "sidekick": {
      "command": "npx",
      "args": ["-y", "@sidekickhero/mcp@latest"]
    }
  }
}
```

**VS Code** — `.vscode/mcp.json`:

```json
{
  "servers": {
    "sidekick": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@sidekickhero/mcp@latest"]
    }
  }
}
```

**Codex** — `~/.codex/config.toml`:

```toml
[mcp_servers.sidekick]
command = "npx"
args = ["-y", "@sidekickhero/mcp@latest"]
tool_timeout_sec = 600
```

> **No tools and no error?** Almost always `npx` not being on PATH — an editor
> launched from the Dock or Spotlight does not read your shell profile, and a
> version manager puts `npx` there. Use the absolute path, which `which npx`
> prints.

## Requirements

- Node 20 or later.
- A Sidekick Hero account with a verified email, for everything except `help`,
  `login`, `logout` and `problem-reports`. The commands' instructions are served
  by Sidekick, so they need a network connection.
- Playwright, only for the commands that drive a browser (`explore`, `flow`,
  `redo-flow`):

  ```sh
  npm i -D playwright && npx playwright install chromium
  ```

## Commands

Every command is `/sidekickhero:<name>`. `/sidekickhero:help` prints this list
from the plugin you have installed, so it is always right for your version.

| Command | Arguments | Needs | What it does |
|---|---|---|---|
| `help` | — | — | Show every Sidekick command and what it needs |
| `login` | — | — | Sign in to your Sidekick account (in a browser) |
| `logout` | — | — | Sign out of Sidekick on this machine |
| `problem-reports` | `[on\|off]` | — | Show or set whether problem reports are sent to the Sidekick team |
| `explore` | `<url>` + options | account, browser | Open the app in a browser and learn how it works |
| `flow` | `"<task>"` + options | account, browser | Create one flow by doing it in the browser |
| `redo-flow` | `"<flow title or id>"` + options | account, browser | Record a saved flow again, replacing its steps in place |
| `update-flow` | `"<flow title or id>" "<what to change>"` | account | Change a saved flow from an instruction, without recording it again |
| `audit` | `[path]` | account | Compare the code with current flows and check for gaps |
| `intro` | `[name]` | account | Write the "What is X?" and "Why X?" guides for this application |
| `features` | `[name]` | account | Write the "Key Features" guide for this application |
| `import` | `<file\|folder\|url>` | account | Import a markdown file, a folder of them, or a web page as guides |
| `welcome-guide` | `[faq\|bot]` | account | Generate the Help Center's Welcome guide |
| `build-help-center` | `[--all] [--style <pack>] [--no-organize] [--context "<how to group>"] [--welcome-guide\|--no-welcome-guide]` | account | Publish the workspace's flows so the help center goes live |
| `organize-help-center` | `[--force] [--apply] [context…]` | account | Organize the help center's flows into categories with AI |
| `build-agent-widget` | `[--apply] [--no-organize] [--context "<how to group>"] [--welcome-guide\|--no-welcome-guide]` | account | Build the agent and add its widget to the app's layout |
| `organize-agent-widget` | `[--apply] [context…]` | account | Organize the agent widget's flows into categories with AI |
| `redo-enhance-images` | `<flow-id>\|--all [--keep-style]` | account | Redraw the placeholders on saved flows with the workspace's current style |
| `backoffice` | `[flows\|help-center\|agent\|home]` | account | Open the Sidekick backoffice (dashboard) in the browser |
| `workspaces` | — | account | List the workspaces on your Sidekick account |
| `workspace` | — | account | Show the current Sidekick workspace |
| `use-workspace` | `<workspace id or name>` | account | Switch to another Sidekick workspace |
| `writing-style` | `[pack\|default]` | account | Show or set the writing style guides and captions are written in |
| `workspace-memory` | `[on\|off]` | account | Use or ignore the workspace memory when writing guides |
| `safety-level` | `[read-only\|safe-forms\|full-auto]` | account | Set what the agent may do while exploring an application |
| `focus-level` | `[original\|highlight\|focus]` | account | Set how much of each screen is masked in recorded flows |
| `logo-rule` | `[on\|off]` | account | Show or set the logo rule (keep the app's logo readable) |

### Options for the recording commands

`explore`, `flow` and `redo-flow` take these; your editor shows them as you type.

| Option | What it does |
|---|---|
| `--url <url>` | Where the flow starts (`flow`, `redo-flow`) |
| `--hint "<text>"` | Where to find the feature, or what changed (`flow`, `redo-flow`) |
| `--window <width>x<height>` | Browser window size; every screenshot is taken at that size |
| `--mobile [device]` | Record the app as a phone shows it (default iPhone 15) |
| `--user-agent "<ua>"` | Send this user agent instead of the device's own |
| `--safety read-only\|safe-forms\|full-auto` | What the agent may do in the app, for this run |
| `--style <pack>` | The writing style for this run's captions |
| `--voice-over [voice]` | Narrate each saved guide — spends AI credits |
| `--memory` / `--no-memory` | Whether the guides' articles use the workspace memory |
| `--no-welcome-guide` | Skip the Welcome guide at the end (`explore`) |
| `--ignore-code` | Explore from the browser alone, without reading the source (`explore`) |

### When an identity provider blocks the browser

Google and some other providers refuse sign-in from an automated browser ("this
browser or app may not be secure"). Attach Sidekick to your own Chrome instead:

```sh
google-chrome --remote-debugging-port=9222 --user-data-dir=/tmp/sk-chrome
```

Sign in to the app in that window, set `SIDEKICK_CDP_URL=http://localhost:9222`
and run the command again. Sidekick drives that session and never closes it.

## Tools

The commands are what you type. These are the tools behind them, which the
agent may also call directly.

| Tool | Needs | What it does |
|---|---|---|
| `analyze_app` | account (for the brief) | Reads the local source: routes, sign-in boundaries, navigation labels, form fields, button text, data the app manages. Signed out, it returns these facts without the brief |
| `verify_ui_strings` | — | Checks a quoted label against the source before it goes into a caption |
| `audit_docs` | account | Compares the workspace's guides, and the repository's markdown, against the code |
| `browser_setup` | Playwright | Install the browser |
| `browser_open`, `browser_await_login` | account, Playwright | Open the app, wait while you sign in to it |
| `browser_snapshot`, `browser_act`, `browser_crawl` | account, Playwright | See the controls, perform a step, map the app's pages |
| `browser_capture`, `browser_close` | account, Playwright | Photograph a step with its target marked; end the session |
| `save_flow`, `list_flows_created` | account | Save a recorded flow to the workspace; list what this session made |
| `list_flows`, `get_flow`, `rename_flow` | account | Find, read and rename saved flows |
| `get_flow_document`, `update_flow_document`, `improve_flow` | account | Read a saved flow's document, save an edited one over it, or have its article rewritten |
| `delete_flow_step`, `delete_flow` | account | Remove one step from a flow; send a flow to the Trash |
| `write_guide`, `import_source`, `welcome_guide` | account | Write a guide from the source, import existing docs, write the Welcome guide |
| `build_help_center`, `organize_help_center` | account | Publish the help center; group its flows into categories |
| `build_agent_widget`, `organize_agent_widget` | account | Build the agent and install its widget; group its flows |
| `redo_enhance_images` | account | Redraw a flow's placeholders in the workspace's current style |
| `sidekick_login`, `sidekick_logout`, `sidekick_whoami` | — | Sign in (in a browser), sign out, show who is signed in |
| `sidekick_workspaces`, `sidekick_workspace`, `sidekick_use_workspace` | account | List, show and switch workspace |
| `sidekick_focus_level`, `sidekick_safety_level`, `sidekick_logo_rule` | account | How much is masked, what the agent may do, whether the logo stays readable |
| `sidekick_workspace_memory`, `sidekick_writing_style` | account | Workspace memory and writing style settings |
| `sidekick_help`, `sidekick_backoffice`, `sidekick_prompt` | — / account | The command list, the dashboard, and a command's instructions |
| `report_problem`, `sidekick_problem_reports` | account / — | Send the Sidekick team a problem in a Sidekick tool; whether that is allowed |

**Labels are quoted, never paraphrased.** Every label `analyze_app` reports
comes with the file and line it was read from, and the agent checks each quoted
string with `verify_ui_strings` before writing it into a guide. What the source
cannot settle is reported as unknown — for instance, a route with no sign-in
check in the source is "unknown", never "public".

## What leaves your machine

- **Reading your code stays local.** `analyze_app` and `verify_ui_strings` read
  the repository on your machine and send none of it anywhere. The brief
  `analyze_app` hands the agent is fetched from Sidekick and filled in on your
  machine.
- **Instructions come from Sidekick.** Each command (except `help`, `login`,
  `logout` and `problem-reports`) fetches its instructions from your Sidekick
  server when it runs, and the browser tools fetch the script they run inside
  the page.
- **What you record goes to your workspace.** Screenshots, captions, the
  addresses of the pages recorded and the guides written from them are saved to
  the Sidekick workspace you are signed in to.
- **Each editor gets its own key.** Signing in stores an editor key in
  `~/.sidekick/`, readable only by you — not your web session. It reaches only
  what the plugin needs (flows and workspace settings, never billing, users or
  API keys), lasts 90 days, and is listed under **Account → Coding agents**,
  where you or an admin can disconnect it. `logout` disconnects it too. The app session the browser uses is kept
  in the project's `.sidekick/` folder, which is added to the project's
  `.gitignore` automatically.
- **Problem reports are off until you say yes.** When something goes wrong
  inside Sidekick, the agent asks you once. If you agree (or later run
  `/sidekickhero:problem-reports on`), it sends the tool, a one-line summary, the
  technical detail (sizes, positions, error text), the plugin and editor
  versions, and the page's address without its query string. Never screenshots,
  never what is typed into forms, and nothing when you are not signed in.
  Reports are kept for 90 days. `SIDEKICK_MCP_TELEMETRY=0` (or `1`) in the
  server's environment decides for every machine without asking.

**Application code is changed in one place only:** `build-agent-widget` adds the
widget to your app's layout file, and it shows you the edit and asks before
making it.

## Configuration

| Variable | What it does |
|---|---|
| `SIDEKICK_SERVER_URL` | Your Sidekick server, for a self-hosted install (default `https://api.sidekickhero.co`) |
| `SIDEKICK_APP_URL` | Your Sidekick web app, for a self-hosted install (default `https://app.sidekickhero.co`) |
| `SIDEKICK_CDP_URL` | Drive your own Chrome instead of launching one (see above) |
| `SIDEKICK_MCP_TELEMETRY` | `0` or `1`: decide problem reports for every machine |

## Licence

Proprietary — Copyright (c) 2026 Sidekick Hero. All rights reserved. You may
install and run the plugin; you may not redistribute it, deobfuscate it, or
build a competing product with it. See [LICENSE](./LICENSE) for the terms, and
your Sidekick Hero agreement for the hosted service the account-based tools
reach. The open-source packages bundled in `dist/` are listed, with their
licences, in [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md).
