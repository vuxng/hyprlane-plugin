---
description: Start work on a Hyprlane issue — read its context and set it In Progress.
argument-hint: "[PGF-123]"
---

Follow the **hyprlane-dev-workflow** skill, "Orient" step, for issue: $ARGUMENTS

- If no identifier was given, first `list_issues` (mine, via `whoami`) and ask
  the user which to start.
- `get_issue` + `get_issue_comments`; pull any linked project/doc.
- `get_team` for the team's status names, then `update_issue` → In Progress.
- Summarize the task and its acceptance criteria, then wait for direction.
