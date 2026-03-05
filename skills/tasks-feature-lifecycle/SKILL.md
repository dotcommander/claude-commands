---
name: tasks-feature-lifecycle
description: |
  Feature lifecycle: requirements, discovery, design approval, quality gates, production.
  Use when: planning feature lifecycle from requirements through design approval to production;
  crystallizing vague requirements into actionable specs.
  NOT for: bug fixes, refactoring, or code review without new feature context.
---
# Feature Development

Guided feature implementation with discovery, design approval, and quality gates.

## Quick Reference

| Intent | Action |
|--------|--------|
| Understand request | Parse requirements, clarify, confirm |
| "Vague request", "make it better" | CLARITY framework: clarify all ambiguities |
| "Break this down" | Task breakdown with milestones |
| Map codebase | Grep for patterns, Glob for files, Read key files |
| Design options | Present 2-3 architectures with trade-offs |
| Implement | Wait for user approval, then edit files |
| Validate | Run tests, lint, build |

**MUST (BLOCKING):**
- Read files before Edit (violate = hallucinated code)
- Run quality checks before Summary (violate = incomplete)

**NEVER:**
- Skip Clarification phase (ambiguity = rework)

**GATE behavior:**
- When running interactively (root agent): pause and ask user at each GATE
- When running as subagent (inside Task): self-assess gates using available context — document assumptions made, flag low-confidence decisions in output for user review

## Workflow

1. **Discovery**: Parse feature request, summarize understanding
   - GATE: user confirms understanding (subagent: document understanding, flag assumptions)
2. **Exploration**: Grep/Glob/Read to map similar features, architecture, integration points
   - 3 parallel searches: similar features, architecture, integration
3. **Clarification**: Identify ALL ambiguities (edge cases, error handling, scope, compatibility)
   - GATE: all questions answered (subagent: list unresolved questions in output, proceed with best-guess defaults)
4. **Design**: Present 2-3 architecture options with trade-offs
   - GATE: user selects approach (subagent: recommend top option with rationale, note alternatives)
5. **Implementation**: Edit files, verify build
   - Interactive: wait for explicit approval first
   - Subagent: proceed with recommended design, document choice
6. **Review**: Check for issues, report at >=80% confidence
   - GATE: user decides which fixes to apply (subagent: apply fixes above 90% confidence, list others)
7. **Summary**: Report files created/modified, key decisions, assumptions made, next steps

## Quality Checklist

```
[ ] All acceptance criteria met
[ ] Tests pass (unit + integration)
[ ] Lint/typecheck pass
[ ] Build succeeds
[ ] No regressions introduced
[ ] Code follows existing patterns
```

## Memory Safety

| Limit | Value | Rationale |
|-------|-------|-----------|
| Files read in exploration | 30 | Focus on relevant patterns |
| Architecture options | 3 | User can't evaluate more |
| Review issues shown | 10 | Prioritize by confidence |

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Skip clarification | Deliver wrong feature — rework required | MUST answer ALL ambiguities before design |
| Implement before approval | User rejects approach — wasted work | Wait for explicit "proceed" after design |
| Too many options | User paralysis — decision fatigue | Present top 2-3 with clear trade-offs |
| Generic exploration | Slow, unfocused — irrelevant results | Use targeted Grep/Glob for discovery |
| Skip quality gates | Broken build — incomplete delivery | Run verification before summary |

## Examples

### Before: Skipping Clarification

```
User: "Add user authentication"
Claude: [immediately starts coding JWT middleware]
Result: Wrong approach - user wanted OAuth, not JWT
```

### After: Enforcing Clarification

```
User: "Add user authentication"
Claude: "Clarifying requirements before design:
  1. Authentication method? (OAuth, JWT, session-based)
  2. User storage? (database, external provider)
  3. Session duration? (remember me, logout behavior)
  4. Password reset flow needed?"
[waits for answers before design phase]
```

## Edge Cases

| Situation | Behavior |
|-----------|----------|
| User unclear on requirements | Ask clarifying questions, don't assume |
| Feature overlaps existing code | Map overlap, propose integration |
| User says "just do it" | Provide recommendation, confirm once |
| Scope creep during implementation | Note additions, ask if should include |
| Existing patterns conflict | Document conflict, ask user preference |

## Success Criteria

- [ ] Requirements confirmed with user (Phase 1 gate)
- [ ] Codebase patterns understood (Phase 2)
- [ ] All ambiguities resolved (Phase 3 gate)
- [ ] Architecture selected by user (Phase 4 gate)
- [ ] Implementation follows chosen design
- [ ] All quality checks pass (Phase 6)
- [ ] Summary report generated (Phase 7)

## Related Skills

- Skill: tasks-spec-methodology - Transform rough notes into developer-ready specs (pre-Phase 1)
- Skill: next-audit-methodology - Deep dive into existing patterns (use before Phase 2)
