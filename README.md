# Claude Commands

Five slash commands that make Claude Code better at debugging, refactoring, planning, and code quality.

Install once, use in any project. Just type the command and Claude handles the rest.

---

## Quick Start

### Option 1: Test locally with --plugin-dir

```bash
git clone https://github.com/dotcommander/claude-commands.git
claude --plugin-dir ./claude-commands
```

Commands are available immediately in that session as `/dc:debug`, `/dc:refactor`, etc.

### Option 2: Install from marketplace

If this plugin is available in your configured marketplace:

```bash
claude plugin install claude-commands

# Or install for your whole team (committed to repo)
claude plugin install claude-commands --scope project
```

### Namespacing

Plugin commands are namespaced to prevent conflicts with other plugins:

| You type | What runs |
|----------|-----------|
| `/dc:debug` | The debug command |
| `/dc:refactor` | The refactor command |
| `/dc:tasks` | The tasks command |
| `/dc:next` | The next command |
| `/dc:learn` | The learn command |

---

## Commands

### /debug

Fixes bugs systematically instead of guessing.

**When to use it:** You have an error message, a failing test, or unexpected behavior and you want a proper diagnosis — not just "try this and see."

```bash
/dc:debug "ImportError: module 'requests' not found"
/dc:debug "Server returns 500 errors intermittently"
/dc:debug "Tests failing after dependency update"
/dc:debug   # no args — Claude examines your current context
```

**What you get:** Claude works through the problem in four stages (Think, Analyze, Plan, Execute), confirms the root cause before touching any code, implements the fix, and runs your tests to verify nothing broke. You get a summary of what was wrong and how it was fixed.

---

### /refactor

Cleans up your code with language-aware improvements.

**When to use it:** Your code works but it's getting messy — duplicated logic, functions doing too much, hard to read. Or you've just finished a feature and want to clean up before committing.

```bash
/dc:refactor              # auto-detect project type and refactor everything
/dc:refactor logging      # focus on a specific area
/dc:refactor performance
/dc:refactor testing
/dc:refactor dry          # DRY optimization + workspace cleanup
/dc:refactor dry src/     # DRY a specific directory
```

**What you get:** Claude detects your project type (Go, JavaScript, PHP, Python, Rust) and applies language-appropriate improvements — extracting duplicated code into reusable functions, simplifying complex logic, improving naming, and removing dead code. In `dry` mode, it also eliminates version sprawl, removes dead code, organizes your workspace, and commits changes in logical groups. You get a list of changes made and any remaining opportunities.

---

### /tasks

Turns rough notes into structured specs with atomic task breakdowns.

**When to use it:** You have a feature idea, a migration plan, or messy notes and you want a clear spec with numbered implementation steps before writing any code.

```bash
/dc:tasks "add caching layer to API"
/dc:tasks "migrate to postgres"
/dc:tasks notes.md
```

**What you get:** A DAWN-structured spec with atomic implementation steps, a verification matrix showing how to prove each step works, and a rollback plan in case things go wrong.

---

### /next

Analyzes your codebase and suggests what to build next.

**When to use it:** You've finished a sprint, are planning the next one, or just want an objective look at what opportunities exist in a codebase — features to add, improvements to make, innovations worth exploring.

```bash
/dc:next                    # analyze current project
/dc:next src/               # focus on a specific directory
/dc:next ~/projects/my-app  # analyze a different project
```

**What you get:** A prioritized roadmap of features, improvements, and innovations — each with a specific file location, rationale, and effort estimate. Parallel scouts analyze different angles and a synthesizer ranks the results by impact.

---

### /learn

Extracts useful insights from documentation you point it at.

**When to use it:** You have a new library to learn, just cloned a vendor package, or want to understand a framework's docs without reading everything yourself.

```bash
/dc:learn docs/languages/go
/dc:learn https://go.dev/doc/go1.26
/dc:learn vendor/package/src --focus="concurrency patterns"
/dc:learn docs/frameworks/svelte --output=~/learnings/svelte.md
/dc:learn compare https://other-project.dev/docs   # compare two sources
```

**What you get:** A structured learning document with insights organized by value — counter-intuitive findings first, then architectural patterns, then implementation details. Training-data-level obvious facts get filtered out so you only see things worth reading.

---

## Plugin Structure

```
claude-commands/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest (name, version, metadata)
├── commands/                  # User-invoked slash commands (thin dispatchers)
│   ├── debug.md
│   ├── refactor.md
│   ├── tasks.md
│   ├── next.md
│   └── learn.md
├── agents/                    # Orchestrator agents (dispatched by commands)
│   ├── debug-agent.md
│   ├── refactor-agent.md
│   ├── tasks-agent.md
│   ├── next-agent.md
│   └── learn-agent.md
├── skills/                    # Agent skills (methodologies loaded by agents)
│   ├── debug-tape-methodology/
│   ├── refactor-code-patterns/
│   ├── tasks-spec-methodology/
│   ├── tasks-feature-lifecycle/
│   ├── next-audit-methodology/
│   └── learn-batch-extraction/
├── CLAUDE.md                  # Architecture documentation
└── README.md
```

**How it works:** Each command is a thin dispatcher (~30 lines) that routes to a specialized agent. The agent loads a skill (methodology) and orchestrates the workflow. Skills can reference additional templates in their `references/` subdirectories.

```
/command → agent → skill → references/
  <50 lines   <200 lines   <500 lines   unlimited
```

---

## Workflows

Commands work well in sequence:

**Fix a bug, then clean up:**
```bash
/dc:debug "error description"
/dc:refactor dry
```

**Full code quality pass:**
```bash
/dc:debug          # fix any issues first
/dc:refactor       # improve code structure
/dc:refactor dry   # eliminate duplication and clean workspace
```

**Learn a new library, then apply it:**
```bash
/dc:learn vendor/new-library/docs --focus="core patterns"
# review the output, then:
/dc:refactor  # apply what you learned
```

---

## Contributing

1. Fork the repo and create a branch
2. Add your command to `commands/` following the existing file format
3. Test it with `claude --plugin-dir ./claude-commands` on a real project
4. Submit a PR with a description of what it does and why it's useful

For architecture details — how commands, agents, and skills fit together — see [CLAUDE.md](CLAUDE.md).

---

## License

MIT — see [LICENSE](LICENSE) for details.

Questions or issues? [Open a GitHub issue](https://github.com/dotcommander/claude-commands/issues).
