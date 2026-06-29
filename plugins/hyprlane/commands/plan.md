---
description: Structure a body of work in Hyprlane — pick the right level (initiative / project / issues) and draft it, following the Linear method.
argument-hint: "<what you want to plan>"
---

Plan and structure this work in Hyprlane, following the **hyprlane-method**
skill: $ARGUMENTS

- **Read context first** — the repo, current files, the user's goal. Ask only
  what context can't answer.
- **Route to the right level** (hyprlane-method's table):
  - one task with a clear outcome → `create_issue` (or `create_sub_issue` under a
    parent).
  - more than ~2 weeks / several people → `create_project` with a named `leadId`
    and a brief spec in its `description`; attach it to an initiative
    (`initiativeId`) if one fits.
  - several projects under one objective → `create_initiative`, then the projects
    beneath it.
- Resolve the target team (`list_teams`) and any parent project/initiative
  (`list_projects` / `list_initiatives`) **before** creating.
- **Propose the structure first** — levels + titles + owner — get the user's nod,
  then create the real entities and report their identifiers. Don't fabricate.
- For a project, offer to capture a short spec doc (`create_doc` →
  `update_doc_body` → `attach_document`).
