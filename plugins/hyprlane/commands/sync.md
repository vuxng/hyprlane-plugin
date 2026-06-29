---
description: Checkpoint the active Hyprlane issue now — post a progress comment and update status.
---

Run the **hyprlane-dev-workflow** "Checkpoint" step for the active issue:

- Ground the note in the actual change — skim `git diff` / what you did since the
  last sync; report what happened, not what you intend to do.
- `comment_issue` a concise progress note (what changed + why).
- `update_issue` status if it changed (pass the lowercase status **key** — e.g.
  `in_progress` — from `get_team`'s workflow states, not the display name).
- If new work surfaced, `create_issue` + `add_issue_relation` to link it.
- Confirm before changing status on an issue you didn't open or that someone
  else owns.
