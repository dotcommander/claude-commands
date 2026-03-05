---
name: tasks-spec-methodology
description: |
  Notes to specs: DAWN structure, verification matrices, rollback plans, architecture decisions.
  Use when: converting rough notes, Slack dumps, or bullet points into structured developer specs;
  writing bug reports, feature specs, CLI tool specs, setup guides, or documentation.
  NOT for: code generation, API reference docs, changelog automation, or inline code comments.
---
# DAWN Spec Methodology

Transform rough inputs into comprehensive, actionable developer specifications using DAWN structure (Diagram, Action, Why-not, Next), an extension of ADR (Architecture Decision Records).

## Quick Reference

| User Says | Template | MUST Include |
|-----------|----------|--------------|
| "document setup", "install guide", "migration plan" | Setup | Complete config files, rollback script |
| "feature spec", "requirements", "new feature" | Feature (references/feature-template.md) | Task DAG, API contracts, test plan |
| "document bug", "root cause", "incident" | Bug (references/bug-template.md) | Reproduction steps, fix verification |
| "CLI tool spec", "agent design", "build a tool" | CLI Tool | Data models, prompts, tasks |
| "quick spec template", "copy-paste ready" | Templates (references/templates.md) | Ready-made templates for common types |

**MUST (BLOCKING)**: Every spec includes all 4 DAWN sections (Diagram, Action, Why-not, Next)

## Spec Structure (DAWN = ADR Extended)

| Phase | ADR Mapping | Purpose | Mandatory |
|-------|-------------|---------|-----------|
| **D**iagram | Context | Visual architecture showing component relationships | Yes |
| **A**ction | Decision | Single copy-paste block to achieve goal | Yes |
| **W**hy-not | Consequences | Gotchas, limitations, alternatives not chosen | Yes |
| **N**ext | Status + Validation | Verification commands, troubleshooting, rollback | Yes |

**ADR Compatibility**: DAWN specs convert to standard ADRs:
- Context = Diagram + problem statement
- Decision = Action (what we chose)
- Status = Proposed/Accepted/Deprecated
- Consequences = Why-not (trade-offs, rejected alternatives)

## Workflow

**Phase 1: Research (MUST before writing)**
1. Classify input type using Quick Reference table
2. Read the matching template from `references/`:
   - Feature → `references/feature-template.md`
   - Bug → `references/bug-template.md`
   - Quick/other → `references/templates.md`
   - Setup/CLI → no template, use DAWN structure directly
3. Discover current state:
   - Glob(pattern="**/*{keyword}*") to find related files
   - Grep(pattern="relevant_term", output_mode="files_with_matches", head_limit=20)
   - Read discovered files to understand architecture

**Phase 2: DAWN Structure (all 4 required)**
4. **Diagram**: ASCII architecture (components + data flow + external deps)
5. **Action**: Complete copy-paste block (NOT snippets)
   - MUST: Pin exact versions tested
   - MUST: Include "if you already have X" paths
6. **Why-not**: Gotchas, limitations, rejected alternatives
7. **Next**: Verification matrix + rollback script

**Phase 3: Validate**
8. Run Checklist Before Delivery (below)
9. If feature/CLI: task DAG with 10-20 min sizing
10. Present spec for approval

## Checklist Before Delivery

**Core Requirements (all specs):**
- [ ] DAWN complete: Diagram + Action + Why-not + Next (all 4 present)
- [ ] Single copy-paste Quick Start block
- [ ] Verification matrix: `| Checkpoint | Command | Expected Output |`
- [ ] Rollback script (bash, executable)
- [ ] Pinned versions tested
- [ ] Zero duplicate content (single source of truth)
- [ ] All file paths verified via Glob/Read (no hallucinated paths)

**Feature/CLI Specs (additional):**
- [ ] Task DAG with 10-20 min sizing per task
- [ ] Dependencies form valid DAG (no cycles)
- [ ] Milestones group related tasks
- [ ] MVP cutoff clear

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Snippets instead of complete files | User must mentally merge | Provide full config files |
| Uncoupled verification | "Check it works" | Every step: Checkpoint + Command + Expected |
| Missing "Why" | User can't adapt | "We chose X because Y. Rejected Z because Q." |
| No rollback | Broken system, no undo | Bash script: uninstall, restore, verify |
| Unpinned versions | Breaks on updates | Pin exact versions tested |
| Research-free writing | Hallucinated paths | MUST: Glob/Grep before writing file paths |

## Examples

### Slack Chat to Setup Spec

**Input:** "We need Caddy for local dev. Also PHP. Make them talk."

**Output:**
```markdown
# Caddy + PHP-FPM Local Setup

## Diagram
DNS (*.test) -> Caddy :80/:443 -> PHP-FPM :9000 -> App

## Action (Quick Start)
brew install caddy php@8.2
brew services start php@8.2
# ... complete config ...

## Why-Not
- VPN blocks .test DNS
- If you have nginx: `brew services stop nginx` first

## Next (Verification)
| Checkpoint | Command | Expected |
|------------|---------|----------|
| PHP running | `lsof -i :9000` | php-fpm listening |
| Caddy running | `curl -I http://app.test` | 200 OK |

Rollback: `brew services stop caddy php@8.2; sudo rm Caddyfile`
```

## Success Criteria

- [ ] User can copy-paste Quick Start and reach working state
- [ ] Every failure mode has a diagnosed command in the Verification Matrix
- [ ] Every component choice has a "Why not X?" alternative documented
- [ ] Rollback script tested and executable
- [ ] All file paths verified via Glob/Read before including in spec

## Related Skills

- Skill: tasks-feature-lifecycle - After spec is approved, drive full feature lifecycle
