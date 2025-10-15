# Claude Commands

A curated collection of powerful commands for [Claude Code](https://claude.ai/code) to enhance your development workflow.

## 📚 Documentation

**Detailed command documentation available in [docs/](docs/)**

Each command has comprehensive documentation including:
- When to use it (specific scenarios and triggers)
- How it works (step-by-step workflow)
- Usage examples (real-world scenarios)
- Integration patterns (command chaining)
- Success criteria (expected outcomes)

Quick links:
- [/ok documentation](docs/ok.md) - Universal router
- [/code/debug documentation](docs/debug.md) - TAPE debugging
- [/code/refactor documentation](docs/refactor.md) - Smart refactoring
- [/code/dry documentation](docs/dry.md) - DRY optimization
- [/code/repomix documentation](docs/repomix.md) - Production audit
- [/learn documentation](docs/learn.md) - Deep learning

---

## 🚀 Commands

### `/ok` - Universal Intelligent Router

**One command that does everything - reads context and routes automatically**

The smart entry point for all workflows. Analyzes your current context (git status, file types, directory structure) and automatically routes to the most appropriate command.

```bash
# Just type /ok and let it figure out what you need
/ok
```

**Smart detection:**
- 🚨 **Errors detected** → Routes to `/code/debug`
- 🔄 **Code smells** → Routes to `/code/refactor`
- 📝 **Duplicate content** → Routes to `/content/dedup` or `/code/dry`
- 📚 **Documentation scattered** → Routes to `/learn` or `/content/write`
- 🎯 **Complex project** → Routes to `/workflow/orchestrate`
- ⚡ **Active development** → Continues current momentum

**Special handling:**
- Detects `~/.claude/*` system directories (commands, agents, hooks)
- Avoids auto-processing command definitions and system files
- Learns your workflow patterns over time

**Philosophy:** "One command, infinite intelligence." Type `/ok` and let context drive action.

---

### `/code/repomix` - Repository Audit Orchestrator

**Automated production readiness audits using AI-powered analysis**

Generates comprehensive audit reports focused on production hardening: caching strategies, observability, security vulnerabilities, and performance optimization.

```bash
# Audit current directory
/code/repomix

# Audit specific project
/code/repomix ~/projects/my-app
```

**What it does:**
1. Auto-detects project type (Go, JavaScript, PHP, Python, Rust)
2. Generates optimized `.repomixignore` for clean code snapshots
3. Uses Gemini AI to analyze codebase for production issues
4. Validates findings with language-specific specialists
5. Outputs single comprehensive `AUDIT.md` with actionable roadmap

**Output:** Prioritized implementation plan with:
- 🚨 Critical (Deploy Blockers)
- ⚠️ High Priority (This Week)
- 📋 Medium Priority (This Month)
- ✅ Quick Wins (Low Effort, High Impact)

**Requirements:**
- [`repomix`](https://github.com/yamadashy/repomix) - `npm install -g repomix`
- [`gemini`](https://github.com/google/generative-ai-go) CLI
- `gemini-specialist` agent (optional, improves quality)

---

### `/code/debug` - Systematic Debugging Workflow

**TAPE methodology enforcement for systematic bug resolution**

Orchestrates error analysis, fix implementation, and validation using the TAPE framework: **Think → Analyze → Plan → Execute**.

```bash
# Debug with error description
/code/debug "ImportError: module 'requests' not found"

# Debug system issue
/code/debug "Server returns 500 errors intermittently"

# General debugging session
/code/debug
```

**TAPE workflow:**
1. **THINK** - Understand system architecture and data flow
2. **ANALYZE** - Gather error symptoms, logs, and evidence
3. **PLAN** - Generate and test hypotheses systematically
4. **EXECUTE** - Implement proven fix, test, and validate

**What it does:**
- Delegates to `error-analysis-specialist` for systematic diagnosis
- Enforces TAPE checkpoints (no Execute without proven root cause)
- Implements fixes via `fix-implementation-specialist`
- Validates with `test-analyzer` (no regressions)
- Extracts debugging patterns for future reference

**Output:** Complete debugging session report with resolution metrics.

**Philosophy:** "Think twice, code once." No trial-and-error. Every fix must be based on proven root cause.

---

### `/code/refactor` - Smart Refactoring Router

**Auto-detects project type and delegates to language-specific specialists**

Intelligent code quality improvements with automatic project detection. Routes to Go, JavaScript, PHP, Python, or Rust specialists based on your codebase.

```bash
# Auto-detect and refactor
/code/refactor

# Focus on specific area
/code/refactor logging
/code/refactor performance
/code/refactor testing
```

**Auto-detection:**
- Checks for `go.mod` → `go-specialist`
- Checks for `package.json` → `js-specialist` (or `react-specialist`, `nextjs-specialist`)
- Checks for `composer.json` → `php-specialist`
- Checks for `requirements.txt`/`pyproject.toml` → Python specialist
- Checks for `Cargo.toml` → Rust specialist

**Refactoring priorities:**
1. **Reusability-first design** - Pure functions, dependency injection, composable code
2. **Simplify** - Reduce complexity, clear naming
3. **Organize** - Proper structure, separation of concerns
4. **Clean** - Remove dead code, eliminate duplication (3+ occurrences → extract)
5. **Clarify** - Self-documenting code, explicit logic

**Output:**
- List of refactorings performed with before/after samples
- Duplication report with extraction recommendations
- Domain-specific utils file suggestions
- Remaining opportunities for future work

**Philosophy:** "Make it better, not perfect - with reusability from day one."

---

### `/learn` - Deep Learning Extraction

**Extract genuinely novel insights from large documentation sets**

Process 1000+ documentation files efficiently using subagent architecture. Filters out "training data synthesis" to surface only counter-intuitive patterns and architectural insights you wouldn't discover otherwise.

```bash
# Learn from Go documentation
/learn docs/languages/go

# Focus on specific topic
/learn vendor/package/src --focus="concurrency patterns"

# Custom output location
/learn docs/frameworks/svelte --output=~/learnings/svelte.md
```

**What it does:**
1. Discovers all documentation files recursively
2. Batches files (10-15 per subagent) for context efficiency
3. Extracts insights using 4-tier novelty classification:
   - **Tier 4** (Counter-intuitive): Contradicts common assumptions
   - **Tier 3** (Architectural): Design philosophy and trade-offs
   - **Tier 2** (Implementation): Technology-specific patterns
   - **Tier 1** (Training data): Excluded automatically
4. Synthesizes findings into structured learning document

**Output:** Organized by value with code examples, anti-patterns, and immediate actions.

**Key benefit:** Can process 500+ files using <50k tokens (10x more efficient than direct reading).

---

### `/code/dry` - DRY Optimization & Workspace Cleanup

**Ruthlessly eliminate redundancy and enforce pristine workspace organization**

Four-phase workflow that transforms messy codebases into maintainable, well-organized projects.

```bash
/code/dry
```

**What it does:**

**Phase 1: DRY Code Optimization**
- Eliminate duplicate code blocks (3+ lines, 2+ occurrences)
- Extract reusable functions and shared modules
- Remove dead code, unused imports, commented-out blocks
- Eliminate version sprawl: `*-v2`, `*-final`, `*-backup` files
- Simplify complexity: flatten nesting, reduce conditions
- Consolidate configuration files

**Phase 2: Test Coverage**
- Run existing tests to verify refactoring
- Add tests for extracted functions
- Target 80% coverage for refactored modules

**Phase 3: Workspace Cleanup**
- Enforce language-specific directory structures (Go, JS, PHP, Python)
- Remove binary artifacts: `*.test`, `*.exe`, `__pycache__/`, `node_modules/`
- Deduplicate and organize documentation into `docs/` subdirectories
- Clean temp files: `.DS_Store`, `*.tmp`, `*.bak`, `*.log`
- Rename files to kebab-case, update imports

**Phase 4: Git Cleanup**
- Logical, atomic commits (refactor → organize → cleanup)
- End with clean working tree: `nothing to commit`

**Philosophy:** Edit files in-place. Use git for version history. One file per purpose. No backup sprawl.

---

## 📦 Installation

### Quick Install

```bash
# Clone repository
git clone https://github.com/dotcommander/claude-commands.git

# Copy to Claude Code commands directory
cp -r claude-commands/code ~/.claude/commands/
cp claude-commands/learn.md ~/.claude/commands/
cp claude-commands/ok.md ~/.claude/commands/

# Verify installation
ls ~/.claude/commands/code/
ls ~/.claude/commands/ok.md
```

### Individual Command Install

```bash
# Universal router (recommended starting point)
curl -o ~/.claude/commands/ok.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/ok.md

# Code commands
curl -o ~/.claude/commands/code/repomix.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/code/repomix.md
curl -o ~/.claude/commands/code/debug.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/code/debug.md
curl -o ~/.claude/commands/code/refactor.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/code/refactor.md
curl -o ~/.claude/commands/code/dry.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/code/dry.md

# Learning command
curl -o ~/.claude/commands/learn.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/learn.md
```

---

## 🎯 Usage Examples

### Universal Router (Start Here!)

```bash
# Let /ok analyze your context and route automatically
/ok

# It detects your situation and picks the right command:
# - Errors? Routes to /code/debug
# - Code smells? Routes to /code/refactor
# - Duplicates? Routes to /code/dry
# - Documentation? Routes to /learn
# - Complex project? Routes to workflow commands
```

### Debugging Workflow (TAPE Methodology)

```bash
# 1. Start systematic debugging
/code/debug "Tests failing after dependency update"

# 2. TAPE phases execute automatically:
#    - THINK: Understand system architecture
#    - ANALYZE: Gather error symptoms and evidence
#    - PLAN: Test hypotheses systematically
#    - EXECUTE: Implement proven fix

# 3. Validation runs automatically (no regressions)

# 4. Review debugging session report
```

### Refactoring Workflow

```bash
# 1. Auto-detect project and refactor
/code/refactor

# 2. Focus on specific improvements
/code/refactor performance
/code/refactor logging

# 3. Clean up workspace after refactoring
/code/dry

# 4. Validate with tests
npm test  # or go test, phpunit, pytest
```

### Production Hardening Workflow

```bash
# 1. Audit codebase
/code/repomix

# 2. Review AUDIT.md and implement Phase 1 critical fixes
/code/debug  # for any issues found

# 3. Refactor based on audit findings
/code/refactor

# 4. Clean up workspace
/code/dry

# 5. Re-audit to verify improvements
/code/repomix
```

### Documentation Learning Workflow

```bash
# 1. Extract insights from new framework documentation
/learn vendor/framework/docs --focus="performance optimization"

# 2. Review learnings-{timestamp}.md for actionable patterns

# 3. Apply Tier 4 (counter-intuitive) insights immediately

# 4. Validate with tests/benchmarks
```

### Complete Code Quality Workflow

```bash
# 1. Let /ok analyze and route
/ok

# 2. Debug any issues first (TAPE methodology)
/code/debug

# 3. Refactor for quality
/code/refactor

# 4. DRY optimization
/code/dry

# 5. Production audit
/code/repomix

# 6. Implement remaining high-priority fixes
/code/refactor

# 7. Final cleanup
/code/dry
```

---

## 🤝 Contributing

We welcome contributions! Here's how:

1. **Fork** this repository
2. **Create a branch** for your command: `git checkout -b add-new-command`
3. **Follow the pattern**: Study existing commands for structure
4. **Test thoroughly**: Ensure your command works in real scenarios
5. **Document clearly**: Include purpose, usage, examples, requirements
6. **Submit PR**: Describe what your command does and why it's useful

### Command Guidelines

- Use frontmatter for metadata (`description`, `argument-hint`, etc.)
- Include clear usage examples
- Document requirements and dependencies
- Explain the "why" not just the "what"
- Provide success criteria and output examples
- Consider error handling and edge cases

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

Built for the [Claude Code](https://claude.ai/code) community.

Special thanks to:
- [repomix](https://github.com/yamadashy/repomix) for repository analysis
- [Google Gemini](https://ai.google.dev/) for AI-powered insights
- The Claude Code team for building an amazing tool

---

**Questions or Issues?** Open an issue on [GitHub](https://github.com/dotcommander/claude-commands/issues).
