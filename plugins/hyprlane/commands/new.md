---
description: Capture a new Hyprlane issue from the current work (a bug or TODO found while coding).
argument-hint: "<title…>"
---

Create an issue capturing: $ARGUMENTS

- **Read context before asking** — the open file, error output, `git diff`, the
  branch name — and draft from that; prompt the user only for what context can't
  supply.
- Write it the Linear way (see **hyprlane-method**): **title = the concrete
  outcome**, short and scannable; description optional and only as long as needed;
  scope to a single outcome, not an epic; no user stories.
- Resolve the target team (`list_teams`; ask if ambiguous), then `create_issue`.
  Set `priority` / `assigneeId` / `projectId` when context makes them clear.
- If there is an active issue in this session, `add_issue_relation` to link the
  new one to it.
- Report the new identifier.
