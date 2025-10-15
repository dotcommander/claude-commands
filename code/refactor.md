---
description: Smart refactoring router with auto-detection and specialist delegation
argument-hint: [focus-area]
allowed-tools: Bash(*), Read, Grep, Glob, Task(language-specialists)
---

# /code/refactor - Smart Refactoring Router

Execute intelligent refactoring with automatic project detection and specialist delegation for: ${ARGUMENTS:-current project}

## 🎯 Command Purpose
**Problem Solved**: Code quality degrades over time without systematic refactoring
**Solution Provided**: Auto-detected, language-specific refactoring via specialist agents
**User Benefit**: Better code without manual pattern lookup or language-specific knowledge

## 🔍 Project Auto-Detection & Specialist Routing

```bash
# Detect project type by checking for marker files
PROJECT_TYPE="unknown"
SPECIALIST="project-analysis-specialist"
FOCUS_AREA="${ARGUMENTS}"

if [ -f "go.mod" ]; then
  PROJECT_TYPE="go"
  SPECIALIST="go-specialist"
  echo "🔍 Detected: Go project"
elif [ -f "package.json" ]; then
  PROJECT_TYPE="javascript"
  SPECIALIST="js-specialist"
  # Check for framework-specific specialists
  if grep -q '"react"' package.json 2>/dev/null; then
    FRAMEWORK_SPECIALIST="react-specialist"
    echo "🔍 Detected: React project"
  elif grep -q '"next"' package.json 2>/dev/null; then
    FRAMEWORK_SPECIALIST="nextjs-specialist"
    echo "🔍 Detected: Next.js project"
  else
    echo "🔍 Detected: JavaScript/TypeScript project"
  fi
elif [ -f "composer.json" ]; then
  PROJECT_TYPE="php"
  SPECIALIST="php-specialist"
  echo "🔍 Detected: PHP project"
elif [ -f "requirements.txt" ] || [ -f "pyproject.toml" ]; then
  PROJECT_TYPE="python"
  SPECIALIST="project-analysis-specialist"  # Fallback until python-specialist exists
  echo "🔍 Detected: Python project"
elif [ -f "Cargo.toml" ]; then
  PROJECT_TYPE="rust"
  SPECIALIST="project-analysis-specialist"  # Fallback until rust-specialist exists
  echo "🔍 Detected: Rust project"
else
  echo "⚠️ Could not auto-detect project type"
  echo "Supported: Go (go.mod), JavaScript (package.json), PHP (composer.json)"
  exit 1
fi

# Verify specialist availability
grep -q "$SPECIALIST" ~/.claude/agents/*.md 2>/dev/null || {
  echo "❌ Required specialist '$SPECIALIST' not available"
  echo "Falling back to: project-analysis-specialist"
  SPECIALIST="project-analysis-specialist"
}

echo "✅ Delegating to: $SPECIALIST"
```

## 🔄 Refactoring Execution

You are now the refactoring engine for a $PROJECT_TYPE project.

Apply intelligent refactoring based on detected project type and focus area: ${FOCUS_AREA:-general improvements}

### REFACTORING PHILOSOPHY

**"Make it better, not perfect" - Focus on practical improvements with clear value**

### CORE REFACTORING PRIORITIES

1. **REUSABILITY-FIRST DESIGN** (SUPREME PRIORITY)
   - **Pure functions**: Transform stateful code into pure functions
   - **Dependency injection**: Replace hard-coded dependencies with injected ones
   - **Single responsibility**: Each function does ONE thing exceptionally well
   - **Composable building blocks**: Small, focused pieces that combine
   - **Detect duplication**: Identify 3+ occurrences for utils extraction
   - **Domain-specific utils**: Suggest {domain}-utils.ts for pure transformations
   - **Clear interfaces**: Define typed contracts for all public APIs
   - **Eliminate global state**: Convert to explicit state management

   **Reusability Checklist (Apply to ALL code):**
   - [ ] Can this be a pure function? (same input → same output)
   - [ ] Are dependencies injected or hard-coded?
   - [ ] Does it have a single, clear responsibility?
   - [ ] Is this composable with other components?
   - [ ] Is this logic duplicated 3+ times? (extract to utils)
   - [ ] Are there hard-coded configs? (inject instead)
   - [ ] Does it use global state? (pass explicitly)

2. **Simplify** - Make code easier to understand
   - Reduce complexity where possible
   - Clear, descriptive naming
   - Extract complex logic into well-named pure functions

3. **Organize** - Put things where they belong
   - Proper file/package structure
   - Logical grouping of related functionality
   - Clear separation of concerns
   - Domain-specific utils files (not generic helpers)

4. **Clean** - Remove dead code and clutter
   - Delete unused imports, variables, functions
   - Remove commented code (preserve in git history)
   - **Eliminate duplicate code** (3+ uses → extract to utils)

5. **Clarify** - Better names, clearer logic
   - Self-documenting code over comments
   - Explicit over clever
   - Consistent naming conventions

REUSABILITY DETECTION & EXTRACTION:
- **Scan for duplicated logic** (3+ occurrences)
- **Identify pure function candidates** (transformations, calculations)
- **Detect hard-coded dependencies** (should be injected)
- **Find global state usage** (should be passed explicitly)
- **Suggest domain-specific utils** when patterns detected:
  * Data transformations → {domain}-utils.ts
  * Business logic → {domain}-logic.ts
  * Format parsing → {format}-parser.ts
  * Calculations → {domain}-calculations.ts

**Decision Tree for Utils Extraction:**
```
Is this logic duplicated 3+ times?
└─ YES → Is it a pure function?
   └─ YES → Is it domain-specific?
      └─ YES → Extract to {domain}-utils.ts
```

LANGUAGE-SPECIFIC IMPROVEMENTS:
$([ "$PROJECT_TYPE" = "go" ] && echo "- **Reusability**: Pure functions, dependency injection via interfaces
- Idiomatic Go patterns (accept interfaces, return structs)
- Proper error handling (never ignore errors)
- Context usage for cancellation
- Table-driven tests
- No global vars (pass dependencies explicitly)")
$([ "$PROJECT_TYPE" = "javascript" ] && echo "- **Reusability**: Pure functions, DI via parameters, composable utilities
- Modern ES6+ patterns
- Eliminate :any types (use proper types with type guards)
- Async/await over promises
- React best practices: pure components, custom hooks for reusable logic
- Domain-specific utils for transformations")
$([ "$PROJECT_TYPE" = "php" ] && echo "- **Reusability**: DI via constructors, interface-driven design, pure functions
- PSR compliance (PSR-12 style, PSR-4 autoloading)
- Type declarations (strict_types, typed properties)
- Dependency injection over globals
- SOLID principles
- Value objects for immutable data")

CRITICAL RULES:
- PRESERVE all functionality (no behavior changes)
- MAINTAIN or improve test coverage
- COMMIT refactorings separately from features
- PROACTIVE analysis - suggest improvements without waiting

FOCUS AREAS (if specified):
- logging: Improve logging statements, consistency, levels
- feature: Refactor for new feature addition
- package: Reorganize package/module structure
- performance: Optimize hot paths
- testing: Improve test structure and coverage

### REFACTORING WORKFLOW

1. **Scan for Code Smells**
   ```bash
   # Find duplicated code
   grep -rn "pattern" src/

   # Identify long functions
   # Check complexity metrics
   ```

2. **Apply Refactorings**
   - Use Read tool to understand current code
   - Use Edit/MultiEdit to apply improvements
   - Follow project conventions
   - Test after each change

3. **Generate Report**

**Example Utils Extraction Report:**
```markdown
## Refactoring Summary

**Project Type**: $PROJECT_TYPE
**Focus Area**: ${FOCUS_AREA:-general improvements}

### Reusability Improvements
- Pure functions extracted: X
- Dependency injection applied: Y locations
- Utils files created: Z

### Duplication Report
DUPLICATION DETECTED: Date formatting logic duplicated 7 times
RECOMMENDATION: Extract to date-utils.ts

// Suggested utils file:
export function formatLifespan(birth: Date | null, death: Date | null): string {
  if (!birth) return 'Unknown';
  if (!death) return `b. ${formatYear(birth)}`;
  return `${formatYear(birth)} - ${formatYear(death)}`;
}

IMPACT: Test once, use everywhere. 7 duplicate test suites → 1 comprehensive suite.

### Refactorings Performed
1. [Refactoring 1 with before/after]
2. [Refactoring 2 with before/after]

### Remaining Opportunities
- [Future improvement 1]
- [Future improvement 2]
```

## Additional Context

### Framework-Specific Patterns

Apply framework-specific best practices based on detection:

**React Projects:**
- Pure components with hooks
- Custom hooks for reusable logic
- Proper dependency arrays
- Memoization for expensive calculations

**Next.js Projects:**
- Server Components vs Client Components
- Proper data fetching patterns
- Route organization
- API route best practices

**Standard JavaScript/TypeScript:**
- Modern ES6+ patterns
- Async/await over promises
- Proper type definitions
- Module organization

## Usage Examples

```bash
# Auto-detect and refactor (recommended)
/code/refactor

# Focus on specific area
/code/refactor logging
/code/refactor performance
/code/refactor testing

# Package reorganization (Go projects)
/code/refactor package
```

## Success Criteria

Operation succeeds when:
- ✅ Project type correctly auto-detected
- ✅ Appropriate specialist successfully invoked
- ✅ Refactorings improve code quality measurably
- ✅ All tests still pass (no behavior changes)
- ✅ Code is more maintainable than before

## Philosophy

**"Make it better, not perfect - with reusability from day one."**

Refactoring is about incremental improvement toward reusable, maintainable code. Focus on practical changes that:
- **Design for reusability** (pure functions, DI, SRP, composable)
- Have clear value
- Don't break existing functionality
- Make future work easier
- Can be reviewed and understood quickly
- Extract duplicated logic to domain-specific utils (3+ occurrences)

Perfect code doesn't exist. Reusable, well-designed code does.

## Reference Documentation

**Complete Reusability Philosophy**: See `~/.claude/CLAUDE.md` § "Reusability-First Design"

Key concepts:
- [The 8 Reusability Principles](~/.claude/CLAUDE.md#the-reusability-mandate)
- [When Utilities Are Perfect](~/.claude/CLAUDE.md#when-utilities-files-are-perfect-domain-specific-helpers) (genealogy example)
- [Decision Tree: Should I Create Utils?](~/.claude/CLAUDE.md#decision-tree-should-i-create-a-utils-file)
- [Language-Specific Patterns](~/.claude/CLAUDE.md#language-specific-reusability-patterns)