# Audit Methodology

Parallel scout architecture for comprehensive codebase analysis.

## Parallel Scout Pattern

**ALL audit modes use parallel scouts -> fuse -> task list**

```
Phase 1: Launch N scouts in parallel via Task (single message)
Phase 2: Each scout returns max 5 findings as JSON
Phase 3: Sonnet fuses, dedupes, ranks findings
Phase 4: Write results to .work/audits/
Phase 5: Present findings for user selection
```

## Context Gathering (Phase 0)

**Quick probe before launching scouts:**

```
Grep("func main|func init", output_mode="content", head_limit=5)
Read(CLAUDE.md)
Read(go.mod OR package.json)
```

**Pass context to each scout in their prompt.**

## Scout Templates by Mode

### Comprehensive (4 scouts)

```
Scout 1 - FLOW: "Find entry points, HTTP handlers, CLI commands.
          Trace call hierarchy 2 levels deep. Return JSON."

Scout 2 - QUERY: "Find database queries, N+1 patterns, missing indexes.
          Check for SQL injection. Return JSON."

Scout 3 - CONCURRENCY: "Find goroutines, channels, mutexes, shared state.
          Check for races, leaks. Return JSON."

Scout 4 - PERFORMANCE: "Find hot paths, allocations, O(n^2) loops.
          Check for bottlenecks. Return JSON."
```

### Flow (3 scouts)

```
Scout 1 - ENTRY: "Find main/init, HTTP handlers, CLI commands"
Scout 2 - CALLS: "From entries, trace outgoing calls 2 levels"
Scout 3 - ABSTRACTIONS: "Find key types, interfaces, patterns"
```

### Roadmap (3 scouts)

```
Scout 1 - FEATURES: "TODOs, incomplete implementations, user-facing gaps"
Scout 2 - IMPROVEMENTS: "Performance bottlenecks, UX friction, error handling"
Scout 3 - INNOVATIONS: "New patterns, better abstractions, integrations"
```

### Security (4 scouts)

```
Scout 1 - INJECTION: "SQL, command, XSS, template injection vectors"
Scout 2 - AUTH: "Broken auth, session handling, privilege escalation"
Scout 3 - DATA: "Sensitive data exposure, validation gaps, crypto"
Scout 4 - INFRA: "Secrets in code, misconfig, vulnerable deps"
```

## Workflow Decision Tree

```
First time on codebase?
+-- Yes -> flow mode (get mental model fast)
|         +-- Then: review mode (get actionable tasks)
+-- No, need specific analysis?
    +-- Performance issues -> comprehensive mode
    +-- Security concerns -> security mode
    +-- Planning features -> roadmap mode
    +-- Tech debt / deps -> unknowns mode
```
