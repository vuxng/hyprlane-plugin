---
name: hyprlane-dev-workflow
description: Use when starting or doing a coding task tracked in Hyprlane, when the user points you at an issue to work on, or when any /hyprlane:* command runs. Encodes the read-context → code → write-progress loop over the hyprlane MCP server.
---

# Hyprlane dev workflow

Run tracked coding work as a loop that reads the issue as the spec and writes
progress back, so Hyprlane stays an accurate project memory. (For the domain
model + tool catalog, see the **using-hyprlane** skill.)

## 1. Orient (read context)

- `whoami` if you don't yet know the user/org.
- Resolve the issue: `get_issue <identifier>` + `get_issue_comments`. Pull any
  linked project/doc for context. **Treat the issue body + acceptance criteria as
  the contract for the work.**
- `get_team` to read the team's workflow states, then `update_issue` →
  In Progress (pass the lowercase status **key** `in_progress`, not the display
  name — see using-hyprlane).

## 2. Work

Write the code, keeping the issue's intent as the spec. If the issue is
ambiguous, ask the user — don't guess and silently diverge.

## 3. Checkpoint (proactive sync)

At natural milestones — a sub-part done, a key decision made, a blocker hit:
- `comment_issue` a concise progress note (what changed + why; not narration).
- `update_issue` status if it changed.
- If new work surfaces (a bug or follow-up found while coding), `create_issue`
  for it and `add_issue_relation` to link it to the current issue.

Sync proactively rather than asking in chat for every write — Claude Code's
per-tool permission prompt is the gate. **But confirm with the user before
changing status on an issue you didn't open or that someone else owns.**

## 4. Finish

- `comment_issue` a closing note: what changed and how it was verified.
- `update_issue` → In Review or Done (pass the status **key** `in_review` /
  `done`, not the display name).
- `add_external_reference` to link the PR / commit / branch.
- `post_project_update` if the work moved a project forward.

## 5. Keep project memory current

Hyprlane is project memory, not just a task list — record durable knowledge so
the next session (yours or a teammate's) can pick up without re-deriving it.

- **Docs** — when you make a non-trivial design decision, write a short spec, or
  land something worth explaining, capture it: `create_doc` then
  `update_doc_body` (plain text; it fails on live-collaborative docs — if so,
  leave that doc to the app). Rename with `update_doc_title`. Mention the doc in
  your issue comment so it's discoverable.
- **Projects** — keep the parent project honest: `update_project` for scope or
  description changes, `post_project_update` to post progress. Use
  `create_project` only when starting a body of work that has none — confirm with
  the user first; projects are shared.
- **Initiatives** — for higher-level rollups, `update_initiative` /
  `post_initiative_update`; usually only when the user asks, not from a single
  coding task.

Same posture as the rest: proactive but non-destructive, and confirm before
editing projects/docs you didn't create.

## Conventions

- Reference real identifiers; never invent issues, comments, or statuses.
- Keep notes factual and terse: what + why + verification.
- **Non-destructive only** — link/comment/update, never delete/archive/bulk.
  If the user needs that, point them to the Hyprlane app.
- Before changing status on an issue you didn't open or that someone else owns,
  **confirm with the user first** — at every step, including Orient.
