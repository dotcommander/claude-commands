# Bug Spec Template

Use this template for bug reports, incidents, and defect documentation.

---

## Template

```markdown
# Bug: [Short Descriptive Title]

**Reporter**: [Who]
**Date**: [When discovered]
**Status**: New | Confirmed | In Progress | Fixed | Closed

---

## TL;DR

| Field | Value |
|-------|-------|
| Severity | Critical / High / Medium / Low |
| Component | [Where in system] |
| Root Cause | [One-line summary] |
| Fix Status | [Pending / PR #X / Deployed] |

---

## Reproduction

### Environment

| Factor | Value |
|--------|-------|
| OS/Browser | [e.g., macOS 14.2 / Chrome 120] |
| App Version | [Version or commit hash] |

### Steps to Reproduce

1. [Prerequisite state]
2. [First action]
3. [Action that triggers bug]

### Expected Behavior
[What should happen]

### Actual Behavior
[What actually happens]

---

## Root Cause Analysis

### Trace

```
Symptom: [What user sees]
    |
    +-- Proximate cause: [Immediate technical cause]
            |
            +-- Root cause: [Fundamental issue]
                    |
                    +-- Location: [file:line]
```

### Technical Explanation
[Why this happens - the code path, data flow, or logic error]

---

## Fix

### Proposed Change

```diff
// [file:line]
- [old code]
+ [new code]
```

### Files Modified

| File | Change | Risk |
|------|--------|------|
| `[path:line]` | [What changed] | Low/Medium/High |

---

## Verification

| Checkpoint | Command | Expected |
|------------|---------|----------|
| Before fix | [command] | [broken output] |
| After fix | [command] | [correct output] |

### Regression Test

```
// Test to add to prevent regression
[test code]
```

---

## Acceptance Criteria

- [ ] Bug no longer reproduces
- [ ] Regression test added
- [ ] Existing tests pass
- [ ] No new warnings/errors in logs
```
