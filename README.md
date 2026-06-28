# Hyprlane — Claude Code plugin

Use [Hyprlane](https://hyprlane.io) — issues, projects, cycles, initiatives, and
docs — directly from Claude Code. This repo is the **distribution marketplace**
for the Hyprlane plugin (the plugin itself lives in
[`plugins/hyprlane`](plugins/hyprlane)).

## Install

```
/plugin marketplace add vuxng/hyprlane-plugin
/plugin install hyprlane@hyprlane
```

Then run any Hyprlane tool (or `/mcp`) — Claude Code opens a one-time browser
OAuth flow. Consent to **`mcp:read`** + **`mcp:write`** and you're done.

> **Note:** you need a Hyprlane account that belongs to an organization. Auth
> succeeds but tools return nothing if your account has no org.

Full setup, smoke test, everyday-use cheatsheet, and troubleshooting:
**[plugins/hyprlane/README.md](plugins/hyprlane/README.md)**.

## What's inside

- **MCP server** `hyprlane` → `https://mcp.hyprlane.io/mcp` — 32 tools (18 read,
  14 non-destructive write).
- **Skills** — `using-hyprlane` (domain model + tool catalog) and
  `hyprlane-dev-workflow` (the read-context → code → write-progress loop).
- **Commands** — `/hyprlane:start`, `/hyprlane:issues`, `/hyprlane:new`,
  `/hyprlane:sync`, `/hyprlane:done`, `/hyprlane:standup`.

## Is it safe that this is public?

Yes. This repo contains **no secrets** — only skill/command markdown, JSON
manifests, and the public MCP URL. Access to your data is gated entirely by
Hyprlane OAuth + organization membership at the server. Installing the plugin
grants nobody access to anything; each user authenticates as themselves.

Destructive actions (delete / archive / bulk) are intentionally **not** exposed
over MCP — do those in the Hyprlane app.

## Source

This is a distribution mirror of the plugin that lives in the Hyprlane monorepo.
Plugin changes are made there and mirrored here.
