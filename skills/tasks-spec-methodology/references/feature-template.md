# Feature Spec Template

Use this template for new features, enhancements, and capability additions.

---

## Template

```markdown
# Feature: [Name]

**Author**: [Who]
**Status**: Draft | Review | Approved | Implemented

---

## TL;DR

| Aspect | Detail |
|--------|--------|
| What | [One sentence describing the feature] |
| Why | [Business value / user benefit] |
| Who | [Primary user/actor] |
| When | [Trigger - what causes this feature to be used] |

---

## Problem Statement

**Current state**: [How things work today]
**Pain point**: [What's broken, missing, or frustrating]
**Impact**: [Quantify if possible]

---

## Proposed Solution

### Architecture

```
[ASCII diagram showing where feature fits in system]
```

**Components affected:**
| Component | Change Type | Description |
|-----------|-------------|-------------|
| `[file/module]` | New | [What's being added] |
| `[file/module]` | Modified | [What's changing] |

---

## User Stories

### US-1: [Primary Use Case]

**As a** [actor/role]
**I want** [capability]
**So that** [benefit]

**Acceptance Criteria:**
- [ ] Given [precondition], when [action], then [result]
- [ ] Given [edge case], when [action], then [expected handling]

---

## Alternatives Considered

| Approach | Pros | Cons | Why Not |
|----------|------|------|---------|
| [Option A] | [Benefits] | [Drawbacks] | [Decision rationale] |
| [Option B] | [Benefits] | [Drawbacks] | [Decision rationale] |

---

## Test Plan

| Scenario | Steps | Expected |
|----------|-------|----------|
| Happy path | [Steps] | [Result] |
| Edge: empty input | [Steps] | [Graceful handling] |
| Error: invalid input | [Steps] | [Error message] |

---

## Rollback Plan

1. [Feature flag]: Set `FEATURE_X_ENABLED=false`
2. [Database]: [Rollback migration if applicable]

---

## Implementation Tasks

Tasks sized for 10-20 minute completion.

### Milestone 1: [Foundation]

| ID | Task | Inputs | Outputs |
|----|------|--------|---------|
| M1.1 | [Create/Add/Implement] [component] | - | `path/to/file` |
| M1.2 | [Next atomic step] | M1.1 | `path/to/file` |
| M1.3 | Write unit tests for [component] | M1.2 | `path/to/test` |

### Milestone 2: [Core Feature]

| ID | Task | Inputs | Outputs |
|----|------|--------|---------|
| M2.1 | [Task description] | M1.3 | `path/to/file` |

### MVP Cutoff

For minimal working feature:
- Complete: M1-MN (X tasks)
- Deferred: Remaining milestones
```

---

## Checklist Before Approval

- [ ] Problem statement is clear and quantified
- [ ] Solution architecture diagram included
- [ ] All user stories have acceptance criteria
- [ ] Test plan covers happy path, edges, and errors
- [ ] Rollback plan exists
- [ ] Implementation tasks are 10-20 minute sized
- [ ] Tasks have clear inputs and outputs
- [ ] MVP cutoff milestone is defined
