# Hyprlane for Claude Code

Operate [Hyprlane](https://hyprlane.io) — issues, projects, cycles, initiatives,
and docs — directly from Claude Code. This plugin wires the Hyprlane MCP server
and teaches Claude Code Hyprlane's domain model and the
read-context → code → write-progress workflow.

## What you get

- **MCP server** `hyprlane` (32 tools: 18 read, 14 non-destructive write) at
  `https://mcp.hyprlane.io/mcp`.
- **Skills** — `using-hyprlane` (domain model + tool catalog) and
  `hyprlane-dev-workflow` (the working loop), auto-loaded when relevant.
- **Commands** — `/hyprlane:start`, `/hyprlane:issues`, `/hyprlane:new`,
  `/hyprlane:sync`, `/hyprlane:done`, `/hyprlane:standup`.

## Prerequisites

- **Claude Code** with plugin support (`/plugin` available).
- **A Hyprlane account that belongs to at least one organization.** Auth
  succeeds but every tool call 401s if you sign in with an account that has no
  org membership — see [Troubleshooting](#troubleshooting).
- A reachable browser for the one-time OAuth consent.

## Install

This repo *is* the plugin marketplace (the `.claude-plugin/marketplace.json` at
its root lists the `hyprlane` plugin). Pick one of the two ways to add it.

### Step 1 — add the marketplace

**A. From GitHub** (recommended — this is a public repo, no auth or checkout needed):

```
/plugin marketplace add vuxng/hyprlane-plugin
```

**B. From a local checkout** (works offline; clone this repo and point at its root,
the folder that contains `.claude-plugin/`):

```
git clone https://github.com/vuxng/hyprlane-plugin
/plugin marketplace add /absolute/path/to/hyprlane-plugin
```

You should see the `hyprlane` marketplace registered with one plugin.

### Step 2 — install the plugin

```
/plugin install hyprlane@hyprlane
```

(The id is `<plugin>@<marketplace>` — both are named `hyprlane`.) This pulls in
the `hyprlane` MCP server, the two skills, and the six commands. Restart Claude
Code if it prompts you to.

### Step 3 — authenticate (one time)

The first Hyprlane tool call returns `401`; Claude Code opens a browser to the
Hyprlane OAuth flow automatically. **Consent to the `mcp:read` and `mcp:write`
scopes.** Tokens refresh automatically (`offline_access`), so this is a one-time
step. You can also trigger it manually by running `/mcp` and authenticating the
`hyprlane` server.

### Step 4 — verify

```
/mcp
```

`hyprlane` should show **connected**. Then ask Claude to "run `whoami`" — it
should return your user and organizations.

## Smoke test

1. `/mcp` → `hyprlane` shows **connected**.
2. Ask: "run `whoami`" → returns your user + organizations.
3. Ask: "list my teams" (`list_teams`) → teams with their issue prefixes.
4. `/hyprlane:start <PGF-123>` → reads the issue and sets it In Progress.
5. `/hyprlane:sync` → posts a progress comment.
6. Open the issue in the Hyprlane app → the comment + status change are there.

## Everyday use

- `/hyprlane:start <ID>` — pull an issue's context and begin (sets In Progress).
- `/hyprlane:issues [mine|team|cycle]` — list issues to pick work from.
- `/hyprlane:new <title>` — capture a bug/TODO found while coding.
- `/hyprlane:sync` — checkpoint: post a progress comment + status now.
- `/hyprlane:done [<ID>]` — closing comment, move to Done/In Review, link the PR.
- `/hyprlane:standup [mine|team]` — a digest from your notifications + activity.

You don't have to use the commands — once installed, Claude Code reads the skills
and operates Hyprlane naturally when you mention an issue (`PGF-123`), a project,
or a doc.

## Updating & removing

- **Update** to the latest plugin version after the marketplace repo changes:
  ```
  /plugin marketplace update hyprlane
  ```
- **Manage / uninstall** from the `/plugin` menu. If the OAuth scopes ever change,
  reconnect the `hyprlane` server from `/mcp` to re-consent.

## Troubleshooting

- **`hyprlane` not connected / tools 401 right after a successful login.** You're
  almost certainly signed in as an account with **no organization membership**.
  The JWT and server are fine; the failure is the membership gate. Fix: sign out
  of `api.hyprlane.io` in the browser (or clear its cookie), reconnect via `/mcp`,
  and sign in with the account that belongs to your Hyprlane org.
- **Browser didn't open for consent.** Run `/mcp` and authenticate the `hyprlane`
  server manually.
- **Marketplace add failed from GitHub.** Check the repo name
  (`vuxng/hyprlane-plugin`); the repo is public, so no auth is needed.
- **A tool says it can't delete/archive.** That's by design — see Safety.

## Safety

Writes are real, immediate, and audited under your identity (`via: "mcp"`).
Claude Code prompts before each tool call unless you allow-list the tools.
**Destructive actions (delete / archive / bulk) are not available over MCP** —
do those in the Hyprlane app.
