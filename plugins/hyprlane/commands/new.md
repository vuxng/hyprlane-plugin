---
description: Capture a new Hyprlane issue from the current work (a bug or TODO found while coding).
argument-hint: "<title…>"
---

Create an issue capturing: $ARGUMENTS

- Draft a clear title and a description from the current context (what, where,
  why it matters).
- Resolve the target team (`list_teams`; ask if ambiguous), then `create_issue`.
- If there is an active issue in this session, `add_issue_relation` to link the
  new one to it.
- Report the new identifier.
