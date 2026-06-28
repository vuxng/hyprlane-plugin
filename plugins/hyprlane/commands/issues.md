---
description: List Hyprlane issues to pick work from (mine / a team / the current cycle).
argument-hint: "[mine|team|cycle]"
---

Show issues to choose from. Scope: $ARGUMENTS (default: mine).

- `whoami` to resolve me, `list_teams` if a team is needed.
- mine → `list_issues` filtered to me as assignee.
- team → `list_issues` for the named/active team.
- cycle → the active cycle's issues (`list_cycles` / `get_cycle`, then `list_issues`).
- Present a short, scannable list (identifier — title — status). Read-only.
