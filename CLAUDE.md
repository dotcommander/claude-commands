# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Claude Code plugin providing five slash commands for debugging, refactoring, planning, learning, and code quality. Distributed via the plugin system (`.claude-plugin/plugin.json`).

**Install:** `claude --plugin-dir ./claude-commands` or `claude plugin install claude-commands`

## Plugin Structure

```
claude-commands/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest (name, version, metadata)
├── commands/                  # Thin dispatchers (~30 lines each)
│   ├── debug.md              # → debug-agent → debug-tape-methodology
│   ├── refactor.md           # → refactor-agent → refactor-code-patterns
│   ├── tasks.md              # → tasks-agent → tasks-spec-methodology
│   ├── next.md               # → next-agent → next-audit-methodology
│   └── learn.md              # → learn-agent → learn-batch-extraction
├── agents/                    # Orchestrators (~80-115 lines each)
│   ├── debug-agent.md
│   ├── refactor-agent.md
│   ├── tasks-agent.md
│   ├── next-agent.md
│   └── learn-agent.md
├── skills/                    # Methodologies (~130-270 lines each)
│   ├── debug-tape-methodology/
│   ├── refactor-code-patterns/
│   ├── tasks-spec-methodology/
│   ├── tasks-feature-lifecycle/
│   ├── next-audit-methodology/
│   └── learn-batch-extraction/
└── README.md
```

## Architecture

### Delegation Chain

```
/command → agent → skill → references/
  <50 lines   <200 lines   <500 lines   unlimited
```

Commands are thin dispatchers that route to agents via `Task()`. Agents load skills via `Skill()` and orchestrate multi-step workflows. Skills contain methodologies and reference templates in `references/` subdirectories.

### Naming Conventions

| Component | Pattern | Example |
|-----------|---------|---------|
| Command | single verb | `debug.md` |
| Agent | `{command}-agent` | `debug-agent.md` |
| Skill | `{command}-{methodology}` | `debug-tape-methodology/SKILL.md` |

### Namespacing

When installed as a plugin, commands are namespaced: `/claude-commands:debug`, `/claude-commands:refactor`, etc.

## Command Patterns

### `/debug` - TAPE Debugging

`commands/debug.md` → `agents/debug-agent.md` → `skills/debug-tape-methodology/`

Think → Analyze → Plan → Execute. Validation gate blocks execution until root cause is proven. Knowledge capture for complex sessions.

### `/refactor` - Auto-Detection Refactoring

`commands/refactor.md` → `agents/refactor-agent.md` → `skills/refactor-code-patterns/`

Auto-detects project type (Go, JS, PHP, Python, Rust). Focus areas: logging, performance, testing, package, dry. DRY mode adds dead code removal, version sprawl elimination, workspace organization.

### `/tasks` - Spec Creation

`commands/tasks.md` → `agents/tasks-agent.md` → `skills/tasks-spec-methodology/` + `skills/tasks-feature-lifecycle/`

DAWN structure (Diagram, Action, Why-not, Next). Verification matrix, rollback plan, atomic task breakdowns (10-20 min sizing).

### `/next` - Codebase Roadmap

`commands/next.md` → `agents/next-agent.md` → `skills/next-audit-methodology/`

Parallel scouts (features, improvements, innovations). Fuses findings into prioritized roadmap. Max 15 findings after dedup. Results to `.work/audits/`.

### `/learn` - Knowledge Extraction

`commands/learn.md` → `agents/learn-agent.md` → `skills/learn-batch-extraction/`

Batch processing with Tier 2-4 novelty classification. Supports directories, files, URLs, and compare mode. Processes 500+ files via parallel subagents.

## Development

### Testing

```bash
# Load plugin locally
claude --plugin-dir ./claude-commands

# Test a command
/claude-commands:debug "test error"

# Load alongside other plugins
claude --plugin-dir ./claude-commands --plugin-dir ./other-plugin
```

### File Format Reference

**Command frontmatter:**
```yaml
---
description: Brief description
argument-hint: <expected-arguments>
allowed-tools: Task, Skill
---
```

**Agent frontmatter:**
```yaml
---
name: agent-name
description: What this agent does
tools: Bash, Edit, Glob, Grep, Read, Write
model: sonnet
permissionMode: acceptEdits
---
```

**Skill frontmatter (SKILL.md):**
```yaml
---
name: skill-name
description: |
  Multi-line description with use-when and not-for guidance.
---
```

### Quality Checklist

- [ ] Command is <50 lines (thin dispatcher only)
- [ ] Agent is <200 lines with Foundation, Workflow, Memory Safety, Anti-Patterns, Success Criteria
- [ ] Skill is <500 lines with Quick Reference, Workflow, Anti-Patterns, Examples, Success Criteria
- [ ] Agent tools list is alphabetized and matches actual usage
- [ ] Heavy content extracted to `skills/{name}/references/`
- [ ] Tested with `claude --plugin-dir` on a real project

## Support

- Issues: https://github.com/dotcommander/claude-commands/issues
- Architecture: Study existing command chains as examples
