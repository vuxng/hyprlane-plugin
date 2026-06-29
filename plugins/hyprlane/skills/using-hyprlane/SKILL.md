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

## Tool catalog (61 — 19 read, 42 non-destructive write)

Read tools require the `mcp:read` scope; write tools require `mcp:write`. Writes
route through the audited `via:"mcp"` services. **There is no delete / archive /
bulk tool** (see Boundaries).

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
- `update_issue` — change title/description/status/priority/assignee/`projectId`/`cycleId`/labels
  (status takes a lowercase **key** like `in_progress`/`done`, not a display name).
- `comment_issue` — add a comment (use for progress notes).
- `update_comment` — edit a comment you authored.
- `add_issue_relation` / `remove_issue_relation` — link / unlink two issues (blocks / blocked_by / related / duplicate).
- `add_external_reference` / `remove_external_reference` — attach / detach a PR/commit/URL.
- `create_sub_issue` — a child issue under a parent (inherits the parent's team).
- `duplicate_issue` — copy an issue.
- `add_co_assignee` / `remove_co_assignee` — add / drop an extra assignee.
- `set_issue_subscription` — subscribe / unsubscribe yourself to an issue.

### Projects — read
- `list_projects` — projects in the org.
- `get_project` — one project's detail.
- `get_project_updates` — a project's status-update history.

### Projects — write
- `create_project` — new project in a team (needs a `teamId`; set `leadId` for the owner, `initiativeId` to nest it).
- `update_project` — edit fields (status / health / description / dates / lead / initiative).
- `post_project_update` — post a status update (health + body).

### Cycles — read
- `list_cycles` — cycles for a team.
- `get_cycle` — one cycle's detail.

### Cycles — write
- `update_cycle` — edit a cycle's name/description.
- `start_cycle` / `complete_cycle` — drive the active cycle's lifecycle.
  (Cycles are **auto-generated from the team cadence** — there is no create-cycle.)

### Initiatives — read
- `list_initiatives` — initiatives in the org.
- `get_initiative` — one initiative's detail.

### Initiatives — write
- `create_initiative` — new initiative.
- `update_initiative` — edit fields (status / health / description / owner / target date).
- `post_initiative_update` — post a status update (health + body).
- `create_sub_initiative` / `create_parent_initiative` — build hierarchy in one step.
- `add_sub_initiative` / `remove_sub_initiative` — link / unlink existing initiatives.

### Documents — read
- `list_documents` — docs in the org.
- `get_document` — one doc's content.

### Documents — write
- `create_doc` — new document (created empty — call `update_doc_body` to set its content).
- `update_doc_title` — rename a document.
- `update_doc_body` — replace a document's body with plain text. Fails on live-collaborative documents.
- `set_doc_visibility` — `org` / `personal`.
- `set_doc_icon` / `set_doc_color` — appearance.
- `move_doc` — reorder among siblings.
- `set_doc_favorite` — pin / unpin in your favorites.

### Resources — read
- `list_resources` — the Documents/Resources panel on a project, initiative, or team.

### Resources — write
- `attach_document` — attach an existing doc to a **project / initiative / team**.
- `attach_link` — attach an external URL (title + url) to a project / initiative / team.

### Teams — write (owner/admin)
- `create_team` — new issue-tracking team (name + `issuePrefix`).
- `update_team` — rename / change prefix / summary.
- `save_team_cadence` — configure cycle cadence (duration, cooldown, start day…).
- `set_team_parent` — nest a team under a parent (or detach).

### Workspace — read
- `list_labels` — labels available in the org.
- `list_members` — org members (for assignee resolution).
- `list_notifications` — my notification inbox.

### Workspace — write
- `mark_notification_read` — mark one notification read.
- `mark_all_notifications_read` — clear the inbox.

## Boundaries

- **Non-destructive only.** There is no **delete, archive, or bulk** tool over
  MCP — by design. (Unlinking a relation/reference, dropping a co-assignee, or
  moving a doc *is* available — those are reversible writes, not deletes.) If the
  user asks to delete or archive an entity, tell them it must be done in the
  Hyprlane app (which has the approval dock and `canDelete`). Don't attempt a
  workaround.
- **Writes are real and audited** as `via: "mcp"` under your identity. Be precise;
  never fabricate identifiers, statuses, comments, or data.
- When **structuring or scoping** work (issue vs project vs initiative, writing
  issues/specs/updates), follow the **hyprlane-method** skill. When you actually
  **do tracked coding work**, follow the **hyprlane-dev-workflow** skill.
