# Directory Structures Reference

Canonical directory structures by project type, docs organization rules, and file cleanup patterns.

---

## Generic Project Structure

```
project/
├── src/            # Source code
│   ├── lib/        # Shared libraries (from DRY refactor)
│   └── utils/      # Utility functions (from DRY refactor)
├── docs/           # Organized documentation (see docs rules below)
│   ├── api/        # API documentation
│   ├── guides/     # How-to guides and tutorials
│   ├── architecture/ # System design docs
│   ├── reference/  # Technical references
│   └── prompts/    # AI prompts and templates
├── scripts/        # Move ALL standalone scripts
├── tests/          # Move ALL test files (unless colocated is standard)
├── config/         # Move ALL config (except .env, package.json, go.mod)
├── examples/       # Usage examples and sample code
├── tmp/            # Create for temporary files
└── [root files]    # ONLY: README.md, CLAUDE.md, LICENSE, package.json, .env, main entry
```

---

## Go Project Structure

```
go-project/
├── cmd/            # Application entrypoints
│   └── myapp/      # Main application
├── internal/       # Private application code
│   ├── services/   # Business logic
│   ├── store/      # Data persistence
│   └── handlers/   # HTTP/gRPC handlers
├── pkg/            # Public libraries
├── docs/           # Documentation
├── scripts/        # Build/deploy scripts
├── migrations/     # Database migrations
├── configs/        # Configuration files
└── [root files]    # go.mod, go.sum, Makefile, README.md, etc.
```

**Go root files (MUST stay in root):**
- go.mod, go.sum — module definition
- Makefile OR Taskfile.yml — pick ONE, not both
- .gitignore, .dockerignore
- Dockerfile, docker-compose.yml
- main.go (if single file) or cmd/ directory

---

## JavaScript/TypeScript Project Structure

```
js-project/
├── src/            # Source code
│   ├── components/ # UI components
│   ├── lib/        # Shared utilities
│   └── utils/      # Helper functions
├── tests/          # Test files (or colocated *.test.ts)
├── docs/           # Documentation
├── scripts/        # Build/deploy scripts
├── public/         # Static assets
└── [root files]    # package.json, tsconfig.json, etc.
```

**JavaScript root files (MUST stay in root):**
- package.json, package-lock.json, yarn.lock, bun.lockb
- tsconfig.json, jsconfig.json
- .eslintrc, .prettierrc, biome.json
- webpack.config.js, vite.config.js, next.config.js

---

## PHP/Laravel Project Structure

```
laravel-project/
├── app/            # Application core
│   ├── Http/       # Controllers, Middleware, Requests
│   ├── Models/     # Eloquent models
│   ├── Services/   # Business logic (from DRY refactor)
│   └── Helpers/    # Helper functions (from DRY refactor)
├── bootstrap/      # Framework bootstrap
├── config/         # Configuration files
├── database/       # Migrations, factories, seeds
├── docs/           # Project documentation
├── public/         # Public assets, index.php
├── resources/      # Views, raw assets, lang files
├── routes/         # Route definitions
├── storage/        # Logs, cache, uploads
├── tests/          # Feature and Unit tests
└── [root files]    # composer.json, artisan, phpunit.xml, etc.
```

**PHP root files (MUST stay in root):**
- composer.json, composer.lock — dependency management
- phpunit.xml, phpstan.neon — testing/analysis config
- .php-cs-fixer.php, phpcs.xml — code style
- artisan (Laravel), index.php — entry point
- .env.example — environment template

---

## Python Project Structure

```
python-project/
├── src/
│   └── mypackage/  # Main package
│       ├── core/   # Core business logic
│       └── utils/  # Utility functions
├── tests/          # Test files
├── docs/           # Documentation
├── scripts/        # Utility scripts
└── [root files]    # pyproject.toml, setup.py, etc.
```

**Python root files (MUST stay in root):**
- requirements.txt, setup.py, pyproject.toml
- Pipfile, Pipfile.lock
- .flake8, .pylintrc, mypy.ini
- manage.py (Django), app.py (Flask)

---

## Rust Project Structure

```
rust-project/
├── src/
│   ├── main.rs     # Binary entrypoint
│   ├── lib.rs      # Library entrypoint
│   └── modules/    # Feature modules
├── tests/          # Integration tests
├── examples/       # Usage examples
├── benches/        # Benchmarks
├── docs/           # Documentation
└── [root files]    # Cargo.toml, Cargo.lock, etc.
```

---

## Docs/ Organization Rules

Documentation must be:
1. **Unique** — no duplicate content across files
2. **Standalone** — each doc self-contained, no circular dependencies
3. **Categorized** — proper subdirectory placement
4. **Named clearly** — descriptive kebab-case names
5. **Deduplicated** — merge overlapping docs before committing

**Deduplication examples:**
- Merge: setup.md + installation.md → getting-started.md
- Combine: api-users.md + api-posts.md → api-reference.md
- Consolidate: deploy-aws.md + deploy-gcp.md → deployment-guide.md

**Canonical docs/ structure:**
```
docs/
├── README.md         # Docs index with navigation
├── api/             # OpenAPI specs, endpoint docs
├── guides/          # Step-by-step tutorials
├── architecture/    # Design decisions, diagrams
├── reference/       # Config options, CLI commands
├── prompts/         # AI prompts and templates
└── archive/         # Outdated docs (before deletion)
```

**Delete these doc anti-patterns:**
- notes.md, misc.md, todo.md, temp.md
- doc1.md, doc2.md, untitled.md
- old-*, backup-*, copy-of-*
- Duplicate content across multiple files

---

## File Cleanup Patterns

### Artifacts to Delete

```bash
# Compiled/generated files:
find . -name "*.pyc" -delete
find . -name "*.pyo" -delete
find . -name "__pycache__" -type d -exec rm -rf {} +
find . -name "*.class" -delete
find . -name "*.o" -delete
find . -name "*.so" -delete
find . -name "*.exe" -delete
find . -name "*.test" -delete    # Go test binaries
find . -name "node_modules" -type d -prune -exec rm -rf {} +
find . -name ".next" -type d -exec rm -rf {} +
find . -name ".nuxt" -type d -exec rm -rf {} +
```

### Language-Specific Test Binaries to Remove

- *.test — Go compiled test binaries (e.g., primary.test)
- coverage.out, coverage.html — test coverage artifacts
- *.prof — profiling data
- *.bench — benchmark results

### Root Directory Audit Checklist

```bash
ls -la | grep -v "^d"  # List all files in root

# Reject:
# Binary files (*.test, *.exe, *.so)
# Duplicate build configs (both Makefile and Taskfile.yml)
# Misplaced documentation (should be in docs/)
# Temporary/backup files (*~, *.bak, *.tmp)
# Version-numbered files (*-v2, *-final, *-old)

# Warn:
# Config sprawl (multiple config files for same purpose)
# Empty directories that should be removed
```

### Workspace Hygiene Checklist

- [ ] No duplicate code blocks remaining
- [ ] No unused dependencies in package.json/go.mod/requirements.txt
- [ ] All tests passing
- [ ] No files in wrong directories
- [ ] No .DS_Store or temp files
- [ ] All imports working
- [ ] Documentation updated and deduplicated
- [ ] Docs properly categorized in subdirs
- [ ] No version-numbered files anywhere
- [ ] No binary files in repository (*.test, *.exe, *.so, etc.)
- [ ] All supporting code in subdirectories
- [ ] No duplicate build systems (Makefile AND Taskfile.yml)
- [ ] Prompts moved to docs/prompts/
- [ ] Config files consolidated (no duplicate configs)
- [ ] Test binaries removed

---

## Git Commit Strategy

Stage changes in logical groups:

```bash
# 1. First commit: DRY refactoring
git add -p src/ lib/ utils/
git commit -m "refactor: eliminate duplication and simplify code"

# 2. Second commit: File organization
git add docs/ scripts/ tests/ config/
git commit -m "chore: organize files into proper directories"

# 3. Third commit: Cleanup
git add -A
git commit -m "chore: remove version sprawl and clean workspace"

# Final verification:
git status  # MUST show: "nothing to commit, working tree clean"
```

If still dirty after commits, review uncommitted changes manually before discarding anything. Use `git status` and `git diff` to understand what remains, then decide whether to stage additional commits or discard changes intentionally.
