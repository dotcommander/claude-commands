---
description: systematic debugging using TAPE methodology - Think, Analyze, Plan, Execute
argument-hint: "<error description>"
allowed-tools: Task, Skill
---

## Dispatch

```
* → Task(debug-agent): debug $ARGUMENTS
```

## Example

```bash
/debug "ImportError: module 'requests' not found"
/debug "Server returns 500 errors intermittently"
/debug "Tests failing after dependency update"
/debug
```

## Success Criteria

- [ ] Root cause identified with concrete evidence
- [ ] Fix implemented and validated
- [ ] No regressions introduced
- [ ] Knowledge captured for complex sessions
