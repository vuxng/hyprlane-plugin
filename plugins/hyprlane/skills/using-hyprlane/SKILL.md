---
name: using-hyprlane
description: Use when working in a project tracked in Hyprlane, when the user mentions Hyprlane or an issue identifier like PGF-123 / a project / cycle / initiative / doc, or when reading or updating Hyprlane context over the hyprlane MCP server. Covers the domain model, identifier scheme, per-team workflow states, and the full tool catalog.
---

# Using Hyprlane

Hyprlane is a Linear-style issue tracker that doubles as **persistent project
memory** you read and write over the `hyprlane` MCP server. Treat it as the
source of truth for what to build and the place to record what you did.

## Domain model

- **Organization** → **Team** → **Issue**. A token can belong to several orgs;
  the org is resolved from your membership (a multi-org `create_doc` /
  `create_initiative` takes an `orgId` to disambiguate, validated against it).
  `list_issues` with no `teamId` aggregates across all your orgs.
- **Team** is the work container. It has an `issuePrefix` (e.g. `PGF`); its issues
  are numbered per team (`PGF-123`). Discover teams with `list_teams`.
- **Issue** — `identifier` (`<PREFIX>-<number>`), title, description, **status**,
  priority, assignee, labels, **relations** (links to other issues), **external
  references** (PR/commit/URL). Comments live on issues.
- **Project / Cycle / Initiative** — planning containers an issue can belong to.
  Projects and initiatives have **updates** (status posts). Cycles are time-boxed.
- **Document** — a rich-text doc (project memory / specs / notes).
- **Notifications** — your inbox of activity.

## Identifiers

Most tools take a human `identifier` (`PGF-123`), not a UUID. `create_issue`
needs a `teamId` — get it from `list_teams`. (`list_issues` takes an *optional*
`teamId`; omit it to list across all your orgs, filtered by assignee/status.)
Never invent an identifier; resolve it first.

## ⚠️ Status: pass the lowercase key, not the display name

Over MCP, `update_issue` accepts `status` as one of six fixed **keys** —
`backlog`, `todo`, `in_progress`, `in_review`, `done`, `canceled` — and rejects
anything else with `Invalid status`. A team's workflow states each carry a
display `name` ("In Progress") **and** a `key` ("in_progress"); `get_team`
returns both. **Pass the `key`, never the display name** — `update_issue` with
status `"In Progress"` or `"Done"` fails; `"in_progress"` / `"done"` is what
works. (Custom, non-canonical team states aren't settable over MCP — do those in
the app.)

## Tool catalog (32)

Read tools require the `mcp:read` scope; write tools require `mcp:write`.

### Discovery (read)
- `whoami` — who am I + which organizations I belong to. Start here.
- `list_teams` — teams across my orgs, with `issuePrefix` and ids. The bootstrap
  for any issue work.
- `get_team` — one team's detail **including its workflow states** (each with a
  `key` to pass when setting issue status) and whether cycles are enabled.

### Issues — read
- `list_issues` — issues for a team, or across all your orgs when `teamId` is omitted (filters: status/assignee/etc.).
- `get_issue` — one issue's full detail (status, priority, assignee, labels,
  relations, external refs) by `identifier`.
- `get_issue_comments` — the comment thread for an issue.

### Issues — write
- `create_issue` — new issue in a team (needs `teamId`).
- `update_issue` — change title/description/status/priority/assignee/etc.
  (status takes a lowercase **key** like `in_progress`/`done`, not a display name).
- `comment_issue` — add a comment (use for progress notes).
- `add_issue_relation` — link two issues (blocks / blocked_by / related / duplicate).
- `add_external_reference` — attach a PR/commit/URL to an issue.

### Projects — read
- `list_projects` — projects in the org.
- `get_project` — one project's detail.
- `get_project_updates` — a project's status-update history.

### Projects — write
- `create_project` — new project.
- `update_project` — edit project fields.
- `post_project_update` — post a project status update.

### Cycles — read
- `list_cycles` — cycles for a team.
- `get_cycle` — one cycle's detail.

### Initiatives — read
- `list_initiatives` — initiatives in the org.
- `get_initiative` — one initiative's detail.

### Initiatives — write
- `create_initiative` — new initiative.
- `update_initiative` — edit initiative fields.
- `post_initiative_update` — post an initiative status update.

### Documents — read
- `list_documents` — docs in the org.
- `get_document` — one doc's content.

### Documents — write
- `create_doc` — new document (created empty — call `update_doc_body` to set its content).
- `update_doc_title` — rename a document.
- `update_doc_body` — replace a document's body with plain text. Fails on live-collaborative documents.

### Workspace (read)
- `list_labels` — labels available in the org.
- `list_members` — org members (for assignee resolution).
- `list_notifications` — my notification inbox.

## Boundaries

- **Non-destructive only.** There is no delete, archive, bulk-update, remove-
  relation, or move tool over MCP — by design. If the user asks to delete or
  archive, tell them it must be done in the Hyprlane app (which has the approval
  dock and `canDelete`). Do not attempt a workaround.
- **Writes are real and audited** as `via: "mcp"` under your identity. Be precise;
  never fabricate identifiers, statuses, comments, or data.
- When you actually do tracked work, follow the **hyprlane-dev-workflow** skill.
