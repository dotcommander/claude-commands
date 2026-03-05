---
name: debug-tape-methodology
description: |
  TAPE systematic debugging methodology: Think, Analyze, Plan, Execute.
  Use when: debugging errors, investigating failures, fixing bugs, diagnosing unexpected behavior.
  NOT for: code review, refactoring, performance profiling unrelated to a bug.
---

# TAPE Debug Methodology

Systematic four-phase debugging that eliminates trial-and-error. Every fix requires a proven root cause.

## Quick Reference

| Intent | Action |
|--------|--------|
| "Bug fix" / "debug" / "error" | Start TAPE from Phase 1 |
| "Can't reproduce" | ANALYZE: gather environment, config, reproduction steps |
| "Intermittent failure" | ANALYZE: add logging, gather timing, check race conditions |
| "Tests failing" | THINK: understand test expectations first |
| "Just try this fix" | STOP — require hypothesis proof before any code change |

**MUST (BLOCKING):**
- Complete Think → Analyze → Plan before writing any code
- Never proceed to Execute with unchecked TAPE validation gate
- Document root cause with evidence before touching files

## Workflow

### Phase 0: Initialize

Create TodoWrite with these 8 phases at session start:

```
1. [ ] Repository context assessment
2. [ ] THINK: system understanding and data flow mapping
3. [ ] ANALYZE: error diagnosis and evidence gathering
4. [ ] PLAN: hypothesis generation and systematic testing
5. [ ] TAPE Validation Gate: verify all phases complete
6. [ ] EXECUTE: fix implementation with proven root cause
7. [ ] EXECUTE: testing and regression validation
8. [ ] Knowledge capture (conditional on complexity)
```

### Phase 1: Context Assessment

Perform lightweight reconnaissance to scope the issue:

- Glob for relevant files (match error symbols, module names)
- Grep for keyword presence across codebase
- Estimate scope (count occurrences)
- Confirm presence/absence of files mentioned in error

**No deep analysis yet.** Output: list of relevant files and scope estimate.

---

### Phase 2: THINK

Understand the system before diagnosing the problem.

- Read architecture files and entry points
- Trace data flow through affected components
- Map dependencies and interactions
- Identify execution paths and state transitions

Document answers:
- How does this component fit in the system?
- What are its dependencies?
- How does data flow through it?

---

### Phase 3: ANALYZE

Gather all evidence systematically.

**Error symptoms:**
- Full stack traces and error messages
- Exact reproduction steps
- Environment details (versions, config, system state)

**Historical context:**
```bash
git log --oneline --since="1 week ago" -- path/to/affected/files | tail -20
git diff HEAD~5..HEAD path/to/affected/files | tail -50
```

Document:
- What exactly happens when the error occurs?
- When did it start happening?
- What changed recently?

---

### Phase 4: PLAN

Generate and systematically disprove hypotheses until root cause is proven.

**Hypothesis template:**
```
Hypothesis N: [Proposed root cause]
- Evidence supporting: [specific observations]
- Test to prove/disprove: [concrete check]
- Result: [Proven / Disproven] — [why]
```

Work through hypotheses until one is **proven**, not just plausible. Document the evidence trail.

---

### Phase 5: TAPE Validation Gate (BLOCKING)

Before writing any code, verify all boxes checked:

- [ ] System deeply understood (THINK complete)
- [ ] All evidence gathered (ANALYZE complete)
- [ ] Root cause PROVEN via tested hypothesis (PLAN complete)
- [ ] No remaining assumptions or guesses
- [ ] Clear fix strategy defined

**If any checkbox unchecked**: return to the incomplete phase. Do NOT proceed.

---

### Phase 6: EXECUTE

Implement the fix based on the proven root cause only.

Steps:
1. Read affected file(s) in full before editing
2. Apply precise changes using Edit (not full rewrites)
3. Follow existing code patterns and conventions
4. Add error handling where appropriate
5. Verify syntax correctness
6. Build/compile if applicable

**Scope discipline:** Fix only the proven root cause. Do not add unrelated improvements.

---

### Phase 7: Testing and Validation

Validate the fix systematically:

```bash
# Scope tests to affected package/module only
go test ./path/to/package/... | tail -30
# or
bun test path/to/affected.test.ts | tail -30
# or
pytest tests/test_affected.py -v | tail -30
```

**Validation checklist:**
- [ ] Original error resolved
- [ ] No new errors introduced
- [ ] All related tests passing
- [ ] Edge cases handled correctly

---

### Phase 8: Knowledge Capture (Conditional)

**Skip for:** simple syntax errors, well-known patterns, trivial typos.

**Document for:** complex integration issues, novel debugging techniques, multi-system coordination, race conditions, non-obvious root causes.

**Pattern template:**
```markdown
## Debugging Pattern: [Error Type]

### Error Signature
- Symptom: [How it manifests]
- Common triggers: [What causes it]
- Affected systems: [Components involved]

### Diagnosis Steps
1. [Investigation step]
2. [Key insight that led to root cause]

### Root Cause
[Detailed explanation]

### Solution
[Fix with code example]

### Prevention
- [Code pattern to avoid recurrence]
- [Early detection signal]

### Similar Issues
- [Related errors with same root cause pattern]
```

## Session Report Template

Generate at end of session:

```markdown
# Debugging Session Report
**Error**: [description]
**Root Cause**: [proven cause]
**Files Modified**: [list]
**Fix Summary**: [brief description]

## TAPE Phase Summary
| Phase | Status |
|-------|--------|
| THINK | complete |
| ANALYZE | complete |
| PLAN | complete — [N] hypotheses tested |
| EXECUTE | complete |
| Validation | PASS / FAIL |
| Pattern Extracted | YES / NO |
```

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Fixing before PLAN complete | Guessing wastes time and introduces new bugs | Prove hypothesis first |
| "Obvious" skips THINK phase | Obvious assumptions are often wrong | Always map system understanding |
| Testing one hypothesis only | First hypothesis is often wrong | Test at least 2-3 before claiming proof |
| Broad multi-file changes in EXECUTE | Hard to isolate regressions | Minimal targeted edits only |
| Skipping Phase 8 for "minor" issues | Patterns recur — miss the learning | Capture if novel technique used |
| TAPE gate treated as optional | Gate exists to prevent premature execution | Gate is hard stop, not suggestion |

## Examples

**TAPE gate enforcement — before/after:**
```
# Bad: skipping to fix
User: "Getting a 500 error on /api/users"
Agent: [edits handler.go immediately]  <- VIOLATION

# Good: gate enforced
User: "Getting a 500 error on /api/users"
Agent: THINK — reads handler, traces request path
       ANALYZE — finds stack trace, checks recent commits
       PLAN — hypothesis: null pointer on missing user record (PROVEN via test)
       Gate: all three phases checked
       EXECUTE — adds nil check at line 47
```

**Knowledge capture decision:**
```
Simple error (skip capture):
  "undefined variable x on line 5" — obvious, no pattern to extract

Complex error (capture):
  "Race condition in cache invalidation under high load" — novel, document timing
  pattern, reproduction script, and fix for future reference
```

## Success Criteria

- [ ] All 8 phases tracked in TodoWrite
- [ ] Root cause proven via tested hypothesis, not assumed
- [ ] Fix is minimal and targeted to proven cause
- [ ] Original error resolved with no regressions
- [ ] Knowledge captured for non-trivial sessions

## Related Skills

- Skill: refactor-code-patterns — post-fix cleanup via `dry` focus area
