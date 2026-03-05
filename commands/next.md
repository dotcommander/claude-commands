---
description: analyze codebase and suggest what to build next - features, improvements, innovations
argument-hint: "[path]"
allowed-tools: Task, Skill
---

## Dispatch

```
* → Task(next-agent): Skill(next-audit-methodology) roadmap + review $ARGUMENTS
```

Spawns parallel scouts for features, improvements, and innovations. Synthesizes into a prioritized roadmap of what to build next.

## Example

```bash
/next                    # analyze current project
/next src/               # focus on a specific directory
/next ~/projects/my-app  # analyze a different project
```

## Success Criteria

- [ ] Codebase analyzed for feature opportunities
- [ ] Findings prioritized by impact and effort
- [ ] Each suggestion includes file:line location and rationale
- [ ] Results written to .work/audits/
