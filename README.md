# Claude Commands Repository

Custom commands for Claude Code to enhance development workflows.

## Available Commands

### `/code/repomix` - Repository Audit Orchestrator

Automated codebase analysis using repomix + gemini for production readiness audits.

**Purpose**: Generate actionable improvement roadmaps focused on production hardening (caching, observability, security, performance).

**Output**: Single comprehensive `AUDIT.md` with validated findings and implementation roadmap.

**Usage**:
```bash
/code/repomix                    # Audit current directory
/code/repomix ~/projects/my-app  # Audit specific project
```

**Requirements**:
- `repomix` - Repository packing tool
- `gemini` - CLI for AI analysis
- `gemini-specialist` agent

---

### `/learn` - Deep Learning Extraction

Context-efficient documentation analysis using subagent architecture to extract genuinely new knowledge.

**Purpose**: Process large documentation sets (1000+ files) and extract only novel, actionable insights.

**Output**: Structured learning document with:
- Counter-intuitive patterns (Tier 4)
- Architectural insights (Tier 3)
- Implementation patterns (Tier 2)

**Usage**:
```bash
/learn docs/languages/go
/learn docs/frameworks/svelte --output=~/learnings/svelte.md
/learn vendor/package/src --focus="concurrency patterns"
```

**Key Features**:
- Filters out training data synthesis (Tier 1)
- Batches files into subagents for context efficiency
- Can process 500+ files with <50k tokens

---

### `/code/dry` - DRY Code Optimization & Workspace Cleanup

Eliminate code duplication through DRY principles, then organize workspace to pristine condition.

**Purpose**: Ruthlessly remove redundancy, clean up version sprawl, and enforce proper directory structure.

**Execution Phases**:
1. **DRY Optimization**: Extract functions, consolidate code, remove dead code
2. **Test Coverage**: Verify nothing broke, enhance coverage
3. **Workspace Cleanup**: Organize directories, remove binaries/temp files
4. **Git Cleanup**: Clean commits with logical grouping

**Key Features**:
- Eliminates version sprawl (no more `-v2`, `-final`, `-backup` files)
- Removes binary artifacts (*.test, *.exe, __pycache__, etc.)
- Enforces language-specific directory structures
- Deduplicates documentation
- Ends with clean git status

---

## Installation

1. Clone this repository:
```bash
git clone https://github.com/dotcommander/claude-commands.git
```

2. Copy commands to your Claude Code commands directory:
```bash
cp -r claude-commands/* ~/.claude/commands/
```

3. Verify installation:
```bash
ls ~/.claude/commands/
```

## Contributing

Contributions are welcome! Please ensure:
- Commands follow established patterns
- Documentation is clear and comprehensive
- Examples demonstrate real-world usage

## License

MIT
