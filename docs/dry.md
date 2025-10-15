# /code/dry - DRY Optimization & Workspace Cleanup

**Ruthlessly eliminate redundancy and enforce pristine workspace organization**

## Overview

The `/code/dry` command transforms messy codebases into maintainable, well-organized projects through a four-phase workflow: DRY code optimization, test coverage, workspace cleanup, and git organization.

## When to Use

Use `/code/dry` when:
- **Code duplication detected** - Same logic repeated 3+ times
- **Workspace is messy** - Files in wrong places, temp files scattered
- **Version sprawl** - Multiple `*-v2`, `*-final`, `*-backup` files
- **After refactoring** - Clean up after code improvements
- **Before code review** - Present clean, organized code
- **Repository organization** - Enforce proper directory structure
- **Git cleanup needed** - Clean working tree before commit

## Four-Phase Workflow

### Phase 1: DRY Code Optimization

**Eliminate redundancy:**
- Find all repeated code blocks (3+ lines, 2+ occurrences)
- Extract reusable functions for repeated logic
- Create shared modules for common utilities
- Consolidate similar implementations
- Abstract patterns instead of copy-paste
- Centralize constants and configuration

**Remove dead code:**
- Delete unreachable code paths
- Strip commented-out code blocks
- Clean unused imports and requires
- Remove files with no references
- Prune unused dependencies

**Eliminate version sprawl:**
```bash
# Files to DELETE:
❌ main-v2.js, main-final.js, main-backup.js
❌ config-simple.json, config-full.json
❌ utils-v1.py, utils-v2.py
❌ *-copy.*, *-backup.*, *-old.*

# Correct approach:
✅ Edit files IN PLACE
✅ Use git for version history
✅ One file per purpose
```

**Simplify complexity:**
- Flatten deep nesting (use early returns)
- Reduce conditions (combine related checks)
- Convert to map/filter/reduce where clearer
- Extract variables for complex expressions
- Break up functions over 50 lines
- Convert promise chains to async/await

### Phase 2: Test Coverage

**Verify refactoring:**
- Run existing tests to ensure nothing broke
- Fix any failing tests before proceeding
- Document intentional behavior changes

**Enhance coverage:**
- Add tests for newly extracted functions
- Cover critical paths first (>20% usage)
- Test edge cases for modified code
- Target 80% coverage for refactored modules

### Phase 3: Workspace Cleanup

**Directory organization:**

Enforces language-specific structures:

**Generic Project:**
```
project/
├── src/            # Source code
│   ├── lib/        # Shared libraries
│   └── utils/      # Utility functions
├── docs/           # Organized documentation
│   ├── api/        # API documentation
│   ├── guides/     # How-to guides
│   └── architecture/ # Design docs
├── scripts/        # Standalone scripts
├── tests/          # Test files
├── config/         # Configuration
└── [root files]    # Only essentials
```

**Go Project:**
```
go-project/
├── cmd/            # Application entrypoints
├── internal/       # Private application code
├── pkg/            # Public libraries
├── docs/           # Documentation
└── [root files]    # go.mod, Makefile, README
```

**File cleanup:**
- Delete: `.DS_Store`, `*.tmp`, `*.bak`, `*~`, `*.log`
- Remove binaries: `*.exe`, `*.test`, `*.so`, `__pycache__/`
- Clean build artifacts: `dist/`, `build/`, `node_modules/`
- Rename to kebab-case, update imports
- Archive outdated files before deletion

**Documentation organization:**
```
docs/
├── README.md       # Docs index
├── api/           # API specs
├── guides/        # Tutorials
├── architecture/  # Design decisions
└── reference/     # Config, CLI docs

# Eliminate:
❌ notes.md, misc.md, temp.md
❌ doc1.md, doc2.md, untitled.md
❌ Duplicate content across files
```

### Phase 4: Git Cleanup

**Logical commits:**
```bash
# Commit 1: DRY refactoring
git add src/ lib/ utils/
git commit -m "refactor: eliminate duplication and simplify code"

# Commit 2: File organization
git add docs/ scripts/ tests/ config/
git commit -m "chore: organize files into proper directories"

# Commit 3: Cleanup
git add -A
git commit -m "chore: remove version sprawl and clean workspace"
```

**Final state:**
```bash
git status
# MUST show: "nothing to commit, working tree clean"
```

## Usage Examples

```bash
# Clean current project
/code/dry

# That's it - fully automated
```

## What to Expect

The command will:

1. **Analyze codebase**
   - Scan for duplication patterns
   - Identify dead code and version sprawl
   - Find misplaced files

2. **Apply DRY principles**
   - Extract repeated logic
   - Create shared utilities
   - Remove redundancy

3. **Run tests**
   - Verify functionality preserved
   - Add coverage for new functions

4. **Organize workspace**
   - Move files to correct directories
   - Remove temp and binary files
   - Clean documentation structure

5. **Clean git state**
   - Create logical commits
   - End with clean working tree
   - Generate summary report

## Output

After completion, you'll receive:

**Cleanup Summary:**
- Lines of code removed: X
- Functions extracted: Y
- Files organized: Z
- Binary files removed: N MB saved
- Test coverage: before → after
- Duplicate blocks eliminated: N
- Root directory files: before → after count

## Execution Rules

1. **DRY first** - Complete code optimization before moving files
2. **Test between phases** - Verify functionality after each change
3. **NO BACKUP FILES** - Use git for history, never create `.backup`
4. **Update imports immediately** - Fix broken imports before next phase
5. **Complete cleanup** - Don't leave temporary files or half-done work
6. **EDIT IN PLACE** - NEVER create versions, ALWAYS modify existing
7. **ONE FILE PER PURPOSE** - No simple/full/complex variants
8. **CLEAN GIT STATUS** - Must end with "nothing to commit"

## When NOT to Use

Skip `/code/dry` for:
- **Active development** - Wait for stable state
- **Experimental code** - Let it mature first
- **External code** - Don't modify vendor dependencies
- **Generated files** - Regenerate instead of editing

## Philosophy

**"Edit files in-place. Use git for version history. One file per purpose. No backup sprawl."**

A clean workspace is a productive workspace. Duplication is technical debt. Organization enables collaboration.

## Integration with Other Commands

```bash
# After refactoring, clean workspace
/code/refactor → /code/dry

# Clean before audit
/code/dry → /code/repomix

# Debug, refactor, clean cycle
/code/debug → /code/refactor → /code/dry

# Complete quality workflow
/code/refactor → /code/dry → /code/repomix → /code/dry
```

## Success Criteria

The cleanup succeeds when:
- ✅ <5% code duplication (measured by tools)
- ✅ All functions <10 cyclomatic complexity
- ✅ Test coverage maintained or improved
- ✅ Zero files in wrong locations
- ✅ Working tree clean ("nothing to commit")
- ✅ Zero modified/untracked files
- ✅ Logical, atomic git commits

## Common Scenarios

### Scenario: Version Sprawl
```
Before: main.js, main-v2.js, main-final.js, main-backup.js
After: main.js (single file, clean history in git)
```

### Scenario: Scattered Documentation
```
Before: notes.md, doc1.md, doc2.md, readme-old.md
After: docs/guides/setup.md, docs/api/reference.md
```

### Scenario: Binary Artifacts
```
Before: *.test, *.exe, __pycache__/, node_modules/ in repo
After: Clean repo, binaries in .gitignore
```

### Scenario: Duplicated Logic
```
Before: Date formatting code in 7 different files
After: Single date-utils.ts with comprehensive tests
```
