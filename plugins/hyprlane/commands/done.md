---
description: Finish a Hyprlane issue — closing comment, move to Done/In Review, link the PR.
argument-hint: "[PGF-123]"
---

Run the **hyprlane-dev-workflow** "Finish" step for issue: $ARGUMENTS
(default: the active issue).

- `comment_issue` a closing note: what changed and how it was verified.
- `get_team`, then `update_issue` → In Review or Done (team's real state name).
- `add_external_reference` to link the PR / commit / branch.
- `post_project_update` if the work moved a project. Confirm before closing an
  issue you didn't open.
