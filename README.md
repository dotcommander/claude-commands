# Claude Commands

A curated collection of powerful commands for [Claude Code](https://claude.ai/code) to enhance your development workflow.

## 🚀 Commands

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

# Verify installation
ls ~/.claude/commands/code/
```

### Individual Command Install

```bash
# Install repomix
curl -o ~/.claude/commands/code/repomix.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/code/repomix.md

# Install learn
curl -o ~/.claude/commands/learn.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/learn.md

# Install dry
curl -o ~/.claude/commands/code/dry.md \
  https://raw.githubusercontent.com/dotcommander/claude-commands/main/code/dry.md
```

---

## 🎯 Usage Examples

### Production Hardening Workflow

```bash
# 1. Audit codebase
/code/repomix

# 2. Review AUDIT.md and implement Phase 1 critical fixes
# ... make fixes ...

# 3. Clean up workspace
/code/dry

# 4. Re-audit to verify improvements
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

### Code Quality Workflow

```bash
# 1. DRY optimization
/code/dry

# 2. Audit for production readiness
/code/repomix

# 3. Implement high-priority fixes

# 4. Final cleanup
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
