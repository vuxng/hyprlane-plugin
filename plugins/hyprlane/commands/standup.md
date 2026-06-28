---
description: Generate a standup digest from Hyprlane (notifications + recent activity).
argument-hint: "[mine|team]"
---

Produce a short standup digest. Scope: $ARGUMENTS (default: mine). Read-only.

- `list_notifications` for recent activity.
- `list_issues` filtered to me (or the named team) for in-flight work.
- Summarize as: Done recently · In progress · Blocked/next. Cite identifiers.
