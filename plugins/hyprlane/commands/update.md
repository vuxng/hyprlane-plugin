---
description: Post a Hyprlane project or initiative status update — health + a short, tweet-length note (the Linear cadence).
argument-hint: "[project-slug | initiative]"
---

Post a status update, following **hyprlane-method**'s update cadence: $ARGUMENTS

- Resolve the target: a project (`list_projects` / `get_project`) or initiative
  (`list_initiatives`). Default to the project tied to the active issue if one's
  in this session.
- Ground it: read the latest update (`get_project_updates`) and recent issue
  activity so the note reflects reality, not a guess.
- Draft a **brief, tweet-length** body — what moved since last time + what's next
  — and pick a **health**: `on_track` / `at_risk` / `off_track`.
- Show the draft + health, confirm, then `post_project_update` (or
  `post_initiative_update`).
