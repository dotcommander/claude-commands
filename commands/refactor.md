---
description: refactor code with auto-detected project type and reusability-first improvements
argument-hint: "[logging|performance|testing|package|dry]"
allowed-tools: Task, Skill
---

## Dispatch

```
* → Task(refactor-agent): refactor $ARGUMENTS
```

## Example

```bash
/refactor                # auto-detect project type, general improvements
/refactor logging        # focus on logging patterns
/refactor performance    # focus on N+1 queries, hot paths
/refactor dry            # DRY optimization + workspace cleanup
/refactor dry src/       # DRY a specific directory
```

## Success Criteria

- [ ] Project type correctly auto-detected
- [ ] Refactorings improve code quality measurably
- [ ] All tests still pass (no behavior changes)
- [ ] Duplication reduced (3+ occurrences extracted to utils)
- [ ] (dry mode) Version sprawl eliminated, workspace clean
