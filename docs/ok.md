# /ok - Universal Intelligent Router

**One command that does everything - reads context and routes automatically**

## Overview

The `/ok` command is your smart entry point for all workflows. It analyzes your current context (git status, file types, directory structure) and automatically determines the best action to take.

## When to Use

Use `/ok` when you:
- **Don't know which command to run** - Let context analysis decide
- **Want automatic workflow detection** - Error detection, code smells, duplication
- **Need quick assessment** - "What needs attention in this project?"
- **Starting a new session** - Get oriented on what to do next
- **Maintaining momentum** - Continue existing development flow

## How It Works

`/ok` performs intelligent context analysis:

1. **Checks git status** - Detects uncommitted changes, conflicts, dirty state
2. **Analyzes file types** - Counts .md files, detects code files (.go, .js, .php, .py)
3. **Detects project type** - Looks for go.mod, package.json, composer.json
4. **Identifies patterns** - Duplicate content, scattered documentation, code smells
5. **Routes automatically** - Delegates to the most appropriate command

## Smart Detection Examples

### Error Detection
```bash
# Git shows conflicts or test failures
/ok
→ Routes to /code/debug for systematic troubleshooting
```

### Code Quality Issues
```bash
# Detects code duplication or complexity
/ok
→ Routes to /code/refactor or /code/dry
```

### Documentation Scattered
```bash
# Multiple .md files in disarray
/ok
→ Routes to /learn or documentation processing
```

### Production Readiness
```bash
# Clean codebase, ready for audit
/ok
→ Routes to /code/repomix for production analysis
```

## Priority Routing Matrix

| Priority | Trigger | Action |
|----------|---------|--------|
| 🚨 **CRITICAL** | Build/test failures, crashes | Debug systematically |
| 🔄 **REFACTOR** | Code smells, complexity | Improve code quality |
| 📝 **CONSOLIDATE** | Duplicate docs/code | Consolidate content |
| 🔍 **AUDIT** | Production readiness check | Comprehensive audit |
| ⚡ **CONTINUE** | Active development | Maintain momentum |

## Special Handling

**System directories** (`~/.claude/*`):
- Avoids auto-processing command definitions in `~/.claude/commands/`
- Respects agent configurations in `~/.claude/agents/`
- Handles hooks carefully in `~/.claude/hooks/`

## Usage Examples

```bash
# Simple - just type /ok
/ok

# Let it analyze and route automatically
# No arguments needed - context is everything
```

## What to Expect

After running `/ok`, you'll see:
1. **Context analysis summary** - What was detected
2. **Routing decision** - Which command was selected
3. **Automatic execution** - The chosen command runs
4. **Next steps** - Recommendations for follow-up actions

## When NOT to Use

Skip `/ok` when you:
- **Know exactly which command you need** - Run it directly
- **Want to override automatic routing** - Use specific command
- **Need precise control** - Explicit is better than implicit

## Philosophy

**"One command, infinite intelligence."**

Type `/ok` and let context drive action. The best command is the one you don't have to remember.

## Quick Override

If `/ok` routes incorrectly, you can always run specific commands directly:
- `/code/debug` - Force debugging mode
- `/code/refactor` - Force code improvements
- `/code/dry` - Force DRY optimization
- `/code/repomix` - Force production audit
- `/learn` - Force documentation learning

## Integration

`/ok` works as the entry point for all command chains:

```bash
# Universal workflow
/ok → (auto-routes) → Follow recommendations

# Can always resume with specific commands after /ok
/ok → /code/debug → /code/refactor → /code/dry
```
