---
description: Checkpoint the active Hyprlane issue now — post a progress comment and update status.
---

Run the **hyprlane-dev-workflow** "Checkpoint" step for the active issue:

- `comment_issue` a concise progress note (what changed + why).
- `update_issue` status if it changed (use the team's real state name from
  `get_team`).
- If new work surfaced, `create_issue` + `add_issue_relation` to link it.
- Confirm before changing status on an issue you didn't open or that someone
  else owns.
