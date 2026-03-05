---
name: refactor-code-patterns
description: |
  Language-aware reusability-first refactoring with DRY optimization: project type detection, language rules, focus areas, duplication extraction, dead code removal, version sprawl elimination, and workspace cleanup.
  Use when: refactoring code for reusability, applying language-specific patterns, scanning for code smells, DRY extraction, eliminating duplication, cleaning workspace, removing version sprawl.
  NOT for: debugging, production audits, or adding new features.
---

# Code Refactor Patterns

Reusability-first refactoring and DRY optimization with auto-detected project type and language-specific rules.

## Quick Reference

| Intent | Action |
|--------|--------|
| "Refactor this code" | Detect project type, apply reusability checklist, scan for smells |
| "Focus on logging/performance/testing" | Apply focus area rules below |
| "DRY this code" / "eliminate duplication" | Apply `dry` focus area — full DRY optimization + workspace cleanup |
| "What language patterns apply?" | Check Language Rules table for detected project type |
| "When to extract to utils?" | 3+ occurrences of same pattern → extract to `{domain}-utils.{ext}` |
| "Remove dead code" | Find unreachable paths, unused imports, orphan files — delete |
| "Eliminate version sprawl" | Delete *-v2, *-final, *-old, *-backup files immediately |
| "Organize workspace" | Apply directory structure from references/directory-structures.md |

**MUST (BLOCKING):**
- Detect project type from marker files before applying any rules
- Refactorings must be behavior-preserving only — never change functionality
- Test after every edit to catch regressions immediately
- Require 3+ occurrences before extracting to shared utils
- Never create backup files — use git history
- Complete all code changes before moving files (DRY first, organize second)

## Workflow

1. **Detect Project Type** — Glob for marker files in order: `go.mod` (Go), `package.json` (JS/TS, check for react/next), `composer.json` (PHP), `requirements.txt`/`pyproject.toml` (Python), `Cargo.toml` (Rust). Fail with clear message if undetected.
2. **Parse Focus Area** — Extract from arguments: `logging`, `performance`, `testing`, `package`, `dry`. Default: general improvements.
3. **Scan for Code Smells** — Identify candidates using targeted Grep:
   - Duplication: find identical or near-identical blocks appearing 3+ times (`Grep` for function signatures, repeated logic patterns)
   - Long functions: find functions exceeding 50 lines
   - Hard-coded dependencies: find constructor calls, direct imports of concrete classes
   - Global state: find module-level mutable variables, singletons
   - For `dry` mode: also scan for dead code (unused exports), version sprawl (`*-v2`, `*-backup`), config sprawl (hardcoded URLs/ports)
   - Apply Language Rules (below) to filter by detected project type
4. **Apply Refactorings** — For each candidate:
   - Read the target file before editing
   - Apply the Reusability Checklist (below) to determine the right refactoring
   - Make one change per Edit (extract function, inject dependency, simplify conditional)
   - Test after each change using project-appropriate test command
   - If 3+ occurrences found: extract to `{domain}-utils.{ext}` in the appropriate directory
5. **Generate Report** — Output structured summary with counts and before/after samples.

## Reusability Checklist

Apply to ALL code during refactoring:

- [ ] Can this be a pure function? (same input → same output)
- [ ] Are dependencies injected or hard-coded?
- [ ] Does it have a single, clear responsibility?
- [ ] Is logic duplicated 3+ times? Extract to `{domain}-utils.{ext}`
- [ ] Are there hard-coded configs? Inject instead.
- [ ] Does it use global state? Pass explicitly.

## Language Rules

| Language | Key Patterns |
|----------|-------------|
| Go | Accept interfaces, return structs. Never ignore errors. Context for cancellation. No global vars. |
| JS/TS | Pure functions, DI via parameters. Eliminate `:any`. Custom hooks for reusable logic (React). |
| PHP | DI via constructors. `strict_types`. PSR-12 style. Value objects for immutable data. SOLID. |
| Python | Pure functions, type hints, dataclasses for value objects. No global mutable state. |
| Rust | Ownership-aware refactoring. Trait-based abstractions. Avoid `unwrap` in library code. |

## Focus Areas

| Mode | Action |
|------|--------|
| `logging` | Normalize log levels, add structured fields, remove debug noise |
| `performance` | Identify N+1 queries, unnecessary allocations, hot-path optimizations |
| `testing` | Table-driven tests, extract test helpers, improve coverage of edge cases |
| `package` | Reorganize module boundaries, eliminate circular deps, group by domain |
| `dry` | Full DRY optimization: duplication extraction, dead code removal, version sprawl elimination, complexity simplification, config consolidation, workspace organization |
| (default) | Full reusability scan: duplication, pure functions, DI, naming, dead code |

## DRY Optimization (dry mode)

When `dry` focus is active, apply these additional phases:

### Duplication Detection
Grep for repeated blocks (3+ lines, 2+ occurrences). Extract to `lib/` or `utils/`. Centralize magic numbers/strings to config.

### Dead Code Removal
Delete unreachable code paths, commented-out blocks, unused imports, orphan files. Run dependency analysis before deleting packages.

### Version Sprawl Elimination
Delete immediately — git provides history:
```
*-v2.*  *-final.*  *-old.*  *-backup.*  *-copy.*  *-new.*  *-temp.*
```

### Complexity Simplification
Flatten deep nesting with early returns/guard clauses. Combine related conditionals. Convert promise chains to async/await. Split functions > 50 lines.

### Configuration Consolidation
Single config location (config/ or .env). Replace inline values with config references. Merge duplicate config files.

### Workspace Organization
Apply directory structure from `references/directory-structures.md` for detected project type. Rename files to kebab-case. Delete artifact files. Update all import paths.

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Changing behavior while refactoring | Introduces bugs, conflates concerns | Refactorings must be behavior-preserving only |
| Creating generic `helpers.ts` | Becomes a junk drawer | Use domain-specific utils: `{domain}-utils.{ext}` |
| Extracting on 1-2 occurrences | Premature abstraction | Require 3+ before extracting to utils |
| Refactoring and fixing bugs in same commit | Hard to review and bisect | Separate commits — refactor first, then fix |
| Skipping tests after each change | Silent regressions accumulate | Test after every edit |
| Applying wrong language patterns | Go idioms in Python, etc. | Detect project type first, apply matching rules |
| Creating *.backup before editing | Bloats repo, defeats git | Use git stash or just edit |
| Leaving version-named files | Version sprawl normalizes chaos | Delete — git has the history |
| Moving files before DRY complete | Breaks imports during refactor | DRY optimization first, then organize |

## Examples

**Reusability checklist applied:**
```
Before: function getUser(id) { return db.query("SELECT * FROM users WHERE id = " + id); }
Checklist:
  - Pure function? No — depends on db (hard-coded)
  - Dependencies injected? No — db is global
  - Single responsibility? Yes
  - Duplicated? Check for 3+ occurrences
Fix: Inject db dependency, parameterize query
After: function getUser(db, id) { return db.query("SELECT * FROM users WHERE id = ?", [id]); }
```

**DRY mode — version sprawl cleanup:**
```
Before:                          After:
project/                         project/
├── utils.js                     └── utils.js  # git log shows full history
├── utils-v2.js
├── utils-final.js
└── utils-backup.js
```

**Focus area routing:**
```
/refactor               → Full reusability scan (default)
/refactor logging       → Normalize log levels, structured fields
/refactor performance   → N+1 queries, allocations, hot paths
/refactor dry           → DRY optimization + workspace cleanup
```

## Success Criteria

- [ ] Project type detected from marker files
- [ ] Language-specific rules applied correctly
- [ ] Focus area applied (or general improvements if unspecified)
- [ ] All duplicated patterns 3+ occurrences extracted
- [ ] All refactorings are behavior-preserving
- [ ] Report generated with before/after samples
- [ ] (dry mode) Version sprawl eliminated, workspace organized, git clean

## Related Skills

- Skill: debug-tape-methodology — systematic debugging when refactoring reveals bugs
