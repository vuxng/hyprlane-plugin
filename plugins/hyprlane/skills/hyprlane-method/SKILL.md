---
name: hyprlane-method
description: Use when structuring or planning work in Hyprlane — deciding whether something is an issue, sub-issue, project, or initiative; writing issues, project specs, or status updates; organizing direction (the Compass / Initiatives layer); or whenever the user asks how to set up, scope, or keep work tidy in Hyprlane. Encodes the Linear method Hyprlane is built on.
---

# Hyprlane method (the Linear way)

Hyprlane is built on the **Linear method**. Follow these conventions so the
tracker stays a clean, readable project memory — not a dumping ground. (Tool
catalog + domain model: **using-hyprlane**. The working loop on a single issue:
**hyprlane-dev-workflow**.)

## The shape of work — Compass → execution

Direction flows top-down; delivery flows bottom-up:

```
Initiative  →  Project  →  Issue   (worked in a Cycle)
```

- **Initiative** — a durable, strategic stream tied to a company objective; the
  always-visible "map" of *why* the important work matters and how it's
  progressing. This is Hyprlane's **Compass** layer. Initiatives are
  workspace-wide. Group projects under one (`create_project` with `initiativeId`,
  or `update_project`); nest big rollups with `create_sub_initiative` /
  `add_sub_initiative`.
- **Project** — a body of work with a clear outcome, sized **~1–3 weeks for 1–3
  people**. Lives in a team (`teamId`), has a **named lead** (`leadId`) and a
  **short spec**, and rolls up to an initiative when one fits.
- **Issue** — one concrete task with a defined, checkable outcome. Lives in
  exactly one team (`PGF-123`). Attach it to a project (`projectId`) and a cycle
  (`cycleId`).
- **Sub-issue** — work too big for one issue but too small for a project
  (`create_sub_issue`).

## Routing: pick the level before you create anything

| It's a… | When | Tool |
|---|---|---|
| **Issue** | a single task with a distinct, checkable outcome | `create_issue` |
| **Sub-issue** | too large for one issue, too small for a project | `create_sub_issue` |
| **Project** | takes more than ~2 weeks, or needs several people | `create_project` |
| **Initiative** | several projects serving one strategic objective | `create_initiative` |

- If it has **no "done" state** (an open-ended discussion, a fuzzy idea), it
  doesn't belong in the tracker — keep it in chat or a doc.
- If a project won't fit in **1–3 weeks**, split it into sequential projects, or
  stage it under an initiative.

## Writing issues

- **Title = the outcome**, short and scannable ("Add rate-limit to `/api/chat`"),
  not a user story. Description is optional — add only the context needed to do
  the work.
- **Scope to a single outcome.** Surface follow-ups as their own issues +
  `add_issue_relation`; don't grow one issue into an epic.
- **No user stories**, and keep product-level UX debate out of the issue — that
  belongs in the project spec.
- Set `priority` / `assigneeId` when known; attach to the right `projectId`.

## Project specs & owners

- Every project gets a **named lead** (`leadId`) and a **brief spec** — 1–2
  pages of the key product + technical decisions, enough for situational
  awareness. Put it in the project `description`, or write `create_doc` →
  `update_doc_body` and `attach_document` it to the project.
- **Aim for brevity** — short specs actually get read.

## Updates — keep direction honest

- Post a **weekly** update on each active project (`post_project_update`) and on
  initiatives (`post_initiative_update`): a **health** signal + a short,
  tweet-length body (what moved since last time + what's next).
- Health keys: **`on_track` / `at_risk` / `off_track`**. An update with no health
  signal is noise — always set one.

## Status & backlog hygiene

- **Todo means "starting within the week."** Everything further out stays in
  **Backlog**; promote a manageable few Backlog → Todo at a time.
- Keep **In Progress to 1–2 issues** — finish before starting more.
- Keep the backlog **manageable** — don't hoard every idea; cancel what you
  won't do.
- Status over MCP takes the lowercase **key**
  (`backlog`/`todo`/`in_progress`/`in_review`/`done`/`canceled`) — see
  **using-hyprlane**.

## Cycles

- Cycles in Hyprlane are **auto-generated from each team's cadence** — you don't
  create them. Assign an issue to the active cycle with `update_issue`
  (`cycleId`); unfinished work rolls forward. Drive the active cycle's lifecycle
  with `start_cycle` / `complete_cycle` when needed.
- Don't overload a cycle; a ~2-week cadence is typical.

## Conduct

- Writes are **real, immediate, and audited** as `via:"mcp"` under your identity
  — be precise, never fabricate ids, statuses, or data.
- **Read context before asking** — the open file, errors, `git diff`, the branch
  name (`pgf-123-…` → identifier) — then prompt only for what context can't give.
- **Confirm before touching work that isn't yours** — changing status/assignee on
  an issue you didn't open, or editing a shared project/initiative/doc.
- **Non-destructive only over MCP** — you can unlink relations/refs and reassign,
  but there is no delete/archive/bulk; those stay in the Hyprlane app.
