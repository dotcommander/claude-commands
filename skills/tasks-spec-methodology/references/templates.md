# Spec Templates

Copy-paste templates for common spec types.

## Bug Fix Spec Template

```markdown
# [Bug Name] Fix Specification

## TL;DR

| Symptom | Root Cause | Fix |
|---------|------------|-----|
| [What user sees] | [Why it happens] | [How to fix] |

## Root Cause Analysis

**File**: `path/to/file:123`

```[language]
// CURRENT (BUGGY):
$result = $this->brokenMethod();

// WHY IT'S WRONG:
// [Explanation]
```

## Fix

```[language]
// FIXED:
$result = $this->workingMethod();
```

## Verification

| Checkpoint | Command | Expected |
|------------|---------|----------|
| Bug gone | [command] | [output] |
| Tests pass | [test command] | All green |

## Acceptance Criteria

- [ ] Bug no longer reproduces
- [ ] Regression test added
- [ ] Existing tests pass
```

---

## Feature Spec Template

```markdown
# [Feature Name] Specification

## Overview
[1-2 sentence description]

## Implementation Priority

| Priority | Task | Effort | File |
|----------|------|--------|------|
| **P0** | [Blocking task] | [X lines] | `file` |
| **P1** | [Core feature] | [X lines] | `file` |
| **P2** | [Enhancement] | [X lines] | `file` |

## Acceptance Criteria

- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]

## Files to Modify

| File | Changes |
|------|---------|
| `path/to/file` | [Description] |
```

---

## Transformation Patterns

### Rough Notes to Structured Requirements

| Pattern | Interpretation |
|---------|----------------|
| "X doesn't work" | Bug report - needs root cause |
| "Add X feature" | Feature request - needs priority |
| "X should do Y" | Behavior change - needs before/after |
| "When X, then Y" | Acceptance criterion |
| "Like X but with Y" | Pattern match + delta |
