---
name: next-audit-methodology
description: |
  Codebase audit: security, debt, quality, prioritized findings, parallel scout architecture.
  Use when: auditing a codebase for security vulnerabilities, technical debt, quality issues,
  or discovering feature opportunities (roadmap mode).
  NOT for: single-file code review, formatting, or documentation writing.
---
# Codebase Audit Methodology

Systematic codebase analysis using parallel scouts, severity scoring, and prioritized findings.

## Quick Reference

| Intent | Action | Mode |
|--------|--------|------|
| "Understand how this works" | flow | Entry points + execution paths |
| "What needs fixing?" | review | Prioritized task list |
| "Is this secure?" | security | Vulnerability scan |
| "What can we build?" | roadmap | Feature opportunities |
| "What don't we know?" | unknowns | Hidden complexity |

**MUST (BLOCKING):**
- Gather context BEFORE spawning scouts (Grep + Read, not blind exploration)
- Respect 20-file memory limit per scout (prevents exhaustion)
- Include file:line locations in ALL findings (actionable requirement)
- Filter output to Critical (15+) and High (10-14) only (noise reduction)

## Decision Matrix

| Scenario | Mode |
|----------|------|
| First time on codebase | `flow` |
| Get actionable tasks | `review` |
| Full architecture docs | (default) |
| Security concerns | `security` |
| Planning features | `roadmap` |

## Parallel Scout Architecture

**ALL modes use: parallel scouts -> fuse -> task list**

| Mode | Scouts | Focus |
|------|--------|-------|
| (default) | 4 | flow, query, concurrency, performance |
| `flow` | 3 | entry, calls, abstractions |
| `review` | 4 | critical, high, compound, refactor |
| `roadmap` | 3 | features, improvements, innovations |
| `security` | 4 | injection, auth, data, infra |
| `unknowns` | 5 | deps, wisdom, ancient, scale, implicit |

## Scout Constraints

| Constraint | Value | Why |
|------------|-------|-----|
| Findings/scout | Max 5 | Prevents context bloat |
| Output format | JSON | Structured for processing |
| Files/scout | Max 20 | Memory safety |

## Scout JSON Schema

Each scout returns this structure (no prose outside JSON):

```json
{
  "findings": [
    {
      "file": "src/handlers/user.go",
      "line": 42,
      "category": "feature|improvement|innovation|security|debt",
      "suggestion": "Add rate limiting to API endpoints",
      "rationale": "No rate limiting exists; vulnerable to abuse under load",
      "impact": "high|medium|low",
      "effort": "small|medium|large"
    }
  ]
}
```

**Rules:**
1. Return JSON only — no prose, no explanations outside JSON
2. Max 5 findings per scout (quality over quantity)
3. Every finding MUST include file and line number
4. Impact = user/business value; Effort = implementation complexity

## Workflow

1. **Context Gathering** — MUST gather context BEFORE spawning scouts
   - Grep(pattern="func main|def main|app.listen", output_mode="files_with_matches", head_limit=10)
   - Read(go.mod|package.json|requirements.txt) for dependencies
   - Read(CLAUDE.md) for project-specific patterns

2. **Mode Selection** — Choose based on Decision Matrix

3. **Scout Deployment** — Spawn 3-5 parallel scouts via Task
   - MUST enforce max 5 findings per scout
   - MUST include file:line in each finding
   - MUST respect 20-file read limit per scout

4. **Fusion** — Synthesize scout outputs
   - Deduplicate overlapping findings
   - Score by severity (data corruption +10, security +10, race +8, resource leak +7)
   - Detect compound issues (multiple findings in same function = x1.5 multiplier)

5. **Filter** — MUST output only Critical (15+) and High (10-14)
   - NEVER report Medium/Low (noise reduction)
   - MUST include file:line and fix suggestion for each

6. **Output** — Write results to .work/audits/

## Priority Scoring (Review Mode)

| Factor | Points |
|--------|--------|
| Data corruption risk | +10 |
| Security vulnerability | +10 |
| Race condition | +8 |
| Resource leak | +7 |
| N+1 query | +6 |
| Hot path inefficiency | +5 |
| Missing error handling | +4 |
| Code duplication | +3 |
| Compound issue | x1.5 |

**Thresholds**: Critical: 15+ | High: 10-14 | Medium: 5-9 | Low: <5

## Roadmap Mode (Scout Templates)

For `/next` command — 3 scouts focused on opportunities:

```
Scout 1 - FEATURES: "What new features would add the most value?
          Look at TODOs, incomplete implementations, user-facing gaps."

Scout 2 - IMPROVEMENTS: "What existing code could be improved?
          Performance bottlenecks, UX friction, error handling gaps."

Scout 3 - INNOVATIONS: "What architectural changes would unlock growth?
          New patterns, better abstractions, integration opportunities."
```

Each returns max 5 suggestions with file:line, rationale, and effort estimate.

## Examples

**Context Gathering (correct):**
```bash
Grep(pattern="func main", type="go") -> finds cmd/server/main.go:15
Read(go.mod) -> sees Gin framework + PostgreSQL
# Now spawn scouts WITH this context
```

**Security Finding (correct):**
```
handlers/user.go:42: SQL injection risk
  db.Exec("SELECT * FROM users WHERE id = " + userID)
  Fix: Use parameterized query -> db.Exec("SELECT * FROM users WHERE id = ?", userID)
  Priority: 20 (security +10, data corruption +10)
```

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Skip context gathering | Generic findings | MUST gather context first (BLOCKING) |
| Read unlimited files | Memory exhaustion | Enforce 20-file limit per scout |
| Report Medium/Low findings | Noise buries signal | Filter to Critical and High only |
| Findings without locations | Can't act on it | MUST include file:line in every finding |
| Vague fix suggestions | "Improve error handling" | Provide exact code change or pattern |
| Single-scout analysis | Limited perspective | Always spawn 3-5 parallel scouts |

## Success Criteria

- [ ] Context gathered before scout deployment
- [ ] Mode selected based on need
- [ ] Memory limits respected (20 files/scout)
- [ ] Findings prioritized by severity
- [ ] Compound issues detected and escalated
- [ ] Results written to .work/audits/

## Related Skills

- Skill: tasks-spec-methodology - Structure findings into actionable specs
- Skill: tasks-feature-lifecycle - Drive roadmap items through implementation
