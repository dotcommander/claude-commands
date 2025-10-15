# /code/repomix - Repository Audit Orchestrator

**Automated production readiness audits using AI-powered analysis**

## Overview

The `/code/repomix` command generates comprehensive audit reports focused on production hardening: caching strategies, observability, security vulnerabilities, and performance optimization. It uses the `repomix` CLI and Gemini AI to analyze your entire codebase.

## When to Use

Use `/code/repomix` when:
- **Preparing for production** - Comprehensive readiness check
- **Post-development** - After feature completion, before deployment
- **Technical debt assessment** - Systematic inventory of issues
- **Security audit needed** - Identify vulnerabilities
- **Performance baseline** - Establish optimization priorities
- **After major refactoring** - Verify improvements
- **Onboarding new developers** - Generate current state report

## Requirements

Before using this command, install:

1. **repomix** - Repository packing tool
   ```bash
   npm install -g repomix
   # or
   brew install repomix
   ```

2. **gemini CLI** - AI analysis tool
   ```bash
   # See: https://github.com/google/generative-ai-go
   ```

## Usage Examples

```bash
# Audit current directory
/code/repomix

# Audit specific project
/code/repomix ~/projects/my-app

# Re-audit after fixes
/code/repomix  # Compare with previous AUDIT.md
```

## How It Works

### Phase 1: Setup & Repository Snapshot

1. **Auto-detect project type**
   - Go (finds `go.mod`)
   - JavaScript (finds `package.json`)
   - PHP (finds `composer.json`)
   - Python (finds `requirements.txt` or `pyproject.toml`)
   - Rust (finds `Cargo.toml`)

2. **Generate optimized `.repomixignore`**
   - Excludes build artifacts, dependencies, tests
   - Language-specific ignore patterns
   - Focuses on source code only

3. **Create repository snapshot**
   - Runs `repomix` to pack codebase
   - Generates `repomix-output.md` with clean source

### Phase 2: AI-Powered Analysis

1. **Gemini AI analysis**
   - Analyzes `repomix-output.md` for production issues
   - Focuses on 4 critical dimensions:
     - **Observability** - Logging, monitoring, tracing
     - **Performance** - Caching, optimization, scalability
     - **Security** - Vulnerabilities, input validation, auth
     - **Reliability** - Error handling, graceful degradation

2. **Finding validation**
   - Verifies file:line references
   - Filters false positives
   - Assesses severity based on production impact

3. **Consolidated output**
   - Single comprehensive `AUDIT.md`
   - Prioritized implementation plan
   - Actionable recommendations with code examples

## Output Files

After completion:

1. **AUDIT.md** - Comprehensive production audit (main deliverable)
2. **repomix-output.md** - Repository snapshot (can be deleted)
3. **.repomixignore** - Optimized ignore patterns (keep for future runs)

## Audit Report Structure

The `AUDIT.md` contains:

### Executive Summary
- Production readiness scores (1-10) for each dimension
- Total findings by severity
- Top 3 production risks
- Estimated effort to production-ready

### Critical Findings (Deploy Blockers)
Priority issues that risk production stability:
- Missing error handling in critical paths
- Security vulnerabilities (SQL injection, XSS)
- Performance bottlenecks (N+1 queries)
- Observability gaps (no structured logging)

### High Priority (This Week)
Production hardening needs:
- Caching layer implementation
- Rate limiting and circuit breakers
- Health checks and readiness probes
- Missing authentication on public endpoints

### Medium Priority (This Month)
Performance and observability improvements:
- Database query optimization
- Distributed tracing setup
- Monitoring dashboard creation
- Configuration reload without restart

### Quick Wins (Low Effort, High Impact)
Easy improvements with immediate value:
- Add structured logging to key operations
- Implement database connection pooling
- Add indexes to foreign keys
- Enable CORS for API endpoints

### Implementation Roadmap
Phased approach:
- **Phase 1: Critical Fixes** (Days 1-2)
- **Phase 2: High Priority** (Week 1)
- **Phase 3: Medium Priority** (Month 1)

## What to Expect

The audit focuses on **production hardening**, not code style:

**✅ Flagged:**
- Missing observability (no logging, metrics, tracing)
- Performance issues (no caching, N+1 queries)
- Security gaps (input validation, authentication)
- Reliability concerns (no error handling, timeouts)

**❌ Not Flagged:**
- Code style issues (use linters for that)
- Missing features (assumes code is complete)
- Architectural philosophy (doesn't critique foundations)
- Incomplete implementations (focuses on what exists)

## Prioritization Criteria

**🚨 Critical (Deploy Blockers):**
- Security vulnerabilities with production impact
- Missing error handling in critical paths
- No observability in production systems
- Performance issues that block deployment

**⚠️ High Priority (This Week):**
- Missing caching for expensive operations
- No rate limiting on public endpoints
- Inadequate monitoring and alerting
- Missing health checks

**📋 Medium Priority (This Month):**
- Database optimization opportunities
- Enhanced logging and tracing
- Configuration management improvements
- Deployment safety measures

**✅ Quick Wins:**
- Low-effort, high-impact improvements
- Single-file changes with clear benefits
- Enable existing features (connection pooling)
- Add missing middleware (CORS, compression)

## After the Audit

1. **Review AUDIT.md**
   ```bash
   cat AUDIT.md
   # Focus on executive summary first
   ```

2. **Start with Phase 1 critical fixes**
   ```bash
   /code/debug  # For error-related issues
   /code/refactor  # For code quality issues
   ```

3. **Track progress**
   - Use TodoWrite for implementation roadmap
   - Check off items as completed
   - Re-run audit to verify improvements

4. **Re-audit after major changes**
   ```bash
   /code/repomix
   # Compare new AUDIT.md with previous version
   # Verify production readiness scores improved
   ```

## When NOT to Use

Skip `/code/repomix` for:
- **Incomplete code** - Wait until feature-complete
- **Experimental projects** - Production audit premature
- **Non-production code** - Scripts, tools, experiments
- **During active development** - Stable state needed

## Philosophy

**"Focus on production impact, not theoretical concerns."**

This audit identifies real deployment risks, not style preferences. Every finding includes:
- Specific file:line location (or marked as systemic)
- Clear production impact explanation
- Actionable fix with code example
- Realistic effort estimate

## Integration with Other Commands

```bash
# Complete production hardening workflow
/code/repomix → /code/debug → /code/refactor → /code/dry → /code/repomix

# Iterative improvement cycle
/code/repomix → Fix Phase 1 → /code/repomix → Fix Phase 2 → /code/repomix

# Post-refactoring validation
/code/refactor → /code/dry → /code/repomix (verify no new issues)
```

## Success Criteria

Audit succeeds when:
- ✅ AUDIT.md generated with comprehensive findings
- ✅ All findings have specific file locations
- ✅ Recommendations are actionable with code examples
- ✅ Roadmap is realistic and prioritized by impact
- ✅ Production readiness scores baseline established

## Common Scenarios

### Scenario: Pre-Production Deployment
```bash
# Comprehensive readiness check
/code/repomix

# Review critical findings
grep -A 20 "## 🚨 CRITICAL" AUDIT.md

# Fix deploy blockers
/code/debug  # Fix each critical issue

# Re-audit to verify
/code/repomix
```

### Scenario: After Major Refactoring
```bash
# Baseline before refactoring
/code/repomix
mv AUDIT.md AUDIT-before.md

# Perform refactoring
/code/refactor → /code/dry

# Verify improvements
/code/repomix
diff AUDIT-before.md AUDIT.md
```

### Scenario: Security Audit
```bash
/code/repomix

# Focus on security findings
grep -B 2 -A 10 "Security" AUDIT.md

# Implement security fixes
# Re-audit to verify all security issues resolved
/code/repomix
```
