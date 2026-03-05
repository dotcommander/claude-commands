---
description: transform rough notes into structured specs with atomic task breakdowns
argument-hint: "<notes|description|topic>"
allowed-tools: Task, Skill
---

## Dispatch

```
* → Task(tasks-agent): $ARGUMENTS
```

Produces a DAWN-structured spec with verification matrix, rollback plan, and numbered atomic implementation steps.

## Example

```bash
/tasks "add caching layer to API"
/tasks "migrate to postgres"
/tasks notes.md
```

## Success Criteria

- [ ] Spec produced with DAWN structure
- [ ] Atomic implementation steps included
- [ ] Verification matrix included
- [ ] Rollback plan specified
