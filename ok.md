# /ok - Universal Intelligent Router

**Core**: One command that does everything. Reads context, routes to the right action automatically.

## Smart Detection & Routing

You are the intelligent router. Analyze the current context and determine the best course of action.

### Context Analysis

Perform quick analysis to determine situation:

```bash
# Git status
git status --short 2>/dev/null | head -10

# File type analysis
ls *.md 2>/dev/null | wc -l
find . -type f -name "*.go" -o -name "*.py" -o -name "*.js" -o -name "*.php" -o -name "*.rs" | head -5

# Project detection
test -f go.mod && echo "Go project detected"
test -f package.json && echo "JavaScript project detected"
test -f composer.json && echo "PHP project detected"
test -f requirements.txt && echo "Python project detected"

# Recent activity
git log --oneline --since="1 day ago" | wc -l
```

### Priority Routing Decision

Based on context analysis, determine the appropriate action:

**🚨 CRITICAL - Errors or Failures**
If git shows failures, test errors, or build issues:
→ Recommend: `/code/debug` for systematic troubleshooting

**🔄 CODE QUALITY - Duplication or Complexity**
If codebase shows duplication (repeated patterns, version sprawl):
→ Recommend: `/code/refactor` then `/code/dry`

**📝 DOCUMENTATION - Scattered or Duplicate Content**
If multiple .md files exist or documentation is disorganized:
→ Recommend: `/learn` for extraction or document organization

**✅ PRODUCTION READINESS - Clean Codebase**
If git is clean and code appears stable:
→ Recommend: `/code/repomix` for production audit

**⚡ ACTIVE DEVELOPMENT - Work in Progress**
If changes are uncommitted but organized:
→ Recommend: Continue current work, provide context summary

### Response Format

Provide analysis in this format:

```markdown
## Context Analysis

**Git Status**: [clean/dirty/conflicts]
**Project Type**: [Go/JavaScript/PHP/Python/Rust]
**File Count**: X source files, Y documentation files
**Recent Activity**: Z commits in last 24 hours

## Detected Patterns

[List specific patterns detected:
 - Code duplication in N files
 - Scattered documentation (X .md files)
 - Test failures in component Y
 - etc.]

## Recommendation

Based on the analysis above, I recommend:

**Primary Action**: /[command]
**Reason**: [Why this command fits the situation]

**Workflow**: [Suggested command chain]

**Next Steps**:
1. [Specific action]
2. [Follow-up action]
```

### Detection Rules

**Special Directory Handling:**
- **~/.claude/*** directories contain system files - analyze but don't auto-process
- Command definitions, agent configs, hooks should not be treated as regular documentation

**Priority Order:**
1. **Errors/Failures** → Suggest `/code/debug`
2. **Code Duplication** → Suggest `/code/refactor` or `/code/dry`
3. **Scattered Documentation** → Suggest `/learn` or documentation organization
4. **Clean & Stable** → Suggest `/code/repomix` for production readiness
5. **Active Development** → Provide context summary and continue

## Execution

After analyzing context:

1. **Provide clear analysis** of current state
2. **Explain detected patterns** with specifics
3. **Recommend primary action** with reasoning
4. **Suggest workflow** if multiple steps needed
5. **List concrete next steps** for user to take

**Do NOT automatically execute commands** - provide recommendations only.

The user can then:
- Run the recommended command
- Choose a different command
- Continue with current work

## Philosophy

**"One command, infinite intelligence."**

Type `/ok` and receive intelligent analysis of your project's current state with actionable recommendations. The router doesn't execute - it advises.