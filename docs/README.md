# Command Documentation

Detailed documentation for each Claude Code command in this repository.

## Quick Links

- **[/ok](ok.md)** - Universal intelligent router (start here!)
- **[/code/debug](debug.md)** - Systematic debugging with TAPE methodology
- **[/code/refactor](refactor.md)** - Smart refactoring with auto-detection
- **[/code/dry](dry.md)** - DRY optimization & workspace cleanup
- **[/code/repomix](repomix.md)** - Production readiness audit
- **[/learn](learn.md)** - Deep learning extraction from documentation

## Documentation Structure

Each command documentation includes:

### 1. Overview
Clear description of what the command does and its purpose.

### 2. When to Use
Specific scenarios and triggers for using the command:
- Problem situations
- Development phases
- Code quality indicators
- Project states

### 3. How It Works
Step-by-step explanation of the command's workflow:
- Phases and stages
- Detection and analysis
- Actions performed
- Output generated

### 4. Usage Examples
Real-world usage patterns with command syntax:
```bash
/command argument
/command --flag="value"
```

### 5. What to Expect
Detailed breakdown of command execution:
- What will happen
- What output you'll see
- How long it takes
- What files are created

### 6. Integration with Other Commands
How commands work together in workflows:
```bash
/command1 → /command2 → /command3
```

### 7. Common Scenarios
Real-world examples with context:
- Problem description
- Command usage
- Expected outcome
- Follow-up actions

## Command Categories

### Entry Point
- **[/ok](ok.md)** - Start here for automatic routing

### Code Quality
- **[/code/debug](debug.md)** - Fix bugs systematically
- **[/code/refactor](refactor.md)** - Improve code quality
- **[/code/dry](dry.md)** - Eliminate redundancy

### Production Readiness
- **[/code/repomix](repomix.md)** - Comprehensive audits

### Learning & Documentation
- **[/learn](learn.md)** - Extract insights from docs

## Reading Order

### New Users
1. Start with **[/ok](ok.md)** - Understand automatic routing
2. Read **[/code/debug](debug.md)** - Learn TAPE debugging
3. Explore other commands as needed

### Debugging Workflow
1. **[/code/debug](debug.md)** - Fix the issue
2. **[/code/refactor](refactor.md)** - Improve the code
3. **[/code/dry](dry.md)** - Clean up workspace

### Production Workflow
1. **[/code/repomix](repomix.md)** - Audit readiness
2. **[/code/debug](debug.md)** - Fix critical issues
3. **[/code/refactor](refactor.md)** - Implement improvements
4. **[/code/dry](dry.md)** - Final cleanup
5. **[/code/repomix](repomix.md)** - Verify improvements

### Learning Workflow
1. **[/learn](learn.md)** - Extract framework insights
2. Review Tier 4 (counter-intuitive) patterns
3. **[/code/refactor](refactor.md)** - Apply learned patterns
4. Validate with tests

## Quick Reference

| Command | Primary Use | Time | Output |
|---------|-------------|------|--------|
| `/ok` | Auto-routing | <1 min | Routes to appropriate command |
| `/code/debug` | Bug fixing | 5-30 min | Debugging session report |
| `/code/refactor` | Code improvement | 10-45 min | Refactoring report |
| `/code/dry` | Workspace cleanup | 5-20 min | Cleanup summary |
| `/code/repomix` | Production audit | 10-30 min | AUDIT.md |
| `/learn` | Documentation insights | 20-60 min | learnings-{timestamp}.md |

## Installation

See main [README.md](../README.md) for installation instructions.

## Contributing

When documenting new commands, follow this structure:
1. Clear overview (what + why)
2. Specific "When to Use" triggers
3. Conceptual "How It Works" (not implementation details)
4. Practical examples with real scenarios
5. Integration patterns with other commands
6. Success criteria and expected outcomes

## Note on Agent References

These documentation files are **agent-agnostic**. While the actual command implementations may delegate to specialized agents or sub-processes, the documentation focuses on:
- **What the command does** (not which agents it uses)
- **When to use it** (problem scenarios)
- **What to expect** (outcomes and outputs)
- **How to integrate it** (workflow patterns)

This approach ensures the documentation remains stable even as underlying implementation details change.
