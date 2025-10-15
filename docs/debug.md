# /code/debug - Systematic Debugging Workflow

**TAPE methodology enforcement for systematic bug resolution**

## Overview

The `/code/debug` command orchestrates a systematic approach to debugging using the TAPE framework: **Think → Analyze → Plan → Execute**. No trial-and-error, no shotgun debugging—only methodical problem-solving.

## When to Use

Use `/code/debug` when you encounter:
- **Build failures** - Compilation errors, missing dependencies
- **Test failures** - Failing unit tests, integration tests
- **Runtime errors** - Crashes, exceptions, unexpected behavior
- **Integration issues** - API failures, database connection problems
- **Performance problems** - Slowness, memory leaks, CPU spikes
- **Mysterious bugs** - "It worked yesterday" situations

## The TAPE Framework

### 1. THINK Phase
**Understand the system:**
- Map component architecture and data flow
- Identify dependencies and interactions
- Understand execution paths
- Consider state management

### 2. ANALYZE Phase
**Gather evidence:**
- Collect error messages, stack traces, logs
- Document reproduction steps
- Examine recent code changes
- Review configuration and environment

### 3. PLAN Phase
**Test hypotheses systematically:**
- Generate 2-3 likely root causes
- Design tests for each hypothesis
- Execute tests methodically
- Document evidence for/against each theory

### 4. EXECUTE Phase
**Implement proven fix:**
- Apply fix for verified root cause
- Validate fix resolves original error
- Test for regressions
- Document solution for future reference

## Usage Examples

```bash
# Debug with specific error
/code/debug "ImportError: module 'requests' not found"

# Debug system issue
/code/debug "Server returns 500 errors intermittently"

# Debug test failures
/code/debug "Tests failing after dependency update"

# General debugging session
/code/debug
```

## What to Expect

The command will guide you through:

1. **Repository context assessment**
   - Analyzes project structure
   - Identifies relevant files
   - Scopes the debugging effort

2. **Systematic error analysis**
   - Deep dive into error symptoms
   - Evidence collection and documentation
   - Hypothesis generation and testing

3. **Fix implementation**
   - Code changes based on proven root cause
   - Maintains existing patterns and conventions
   - Complete solution, not partial patches

4. **Testing and validation**
   - Verifies fix resolves original error
   - Checks for regression issues
   - Confirms system stability

5. **Session documentation**
   - Complete debugging report
   - Resolution metrics
   - Knowledge capture for future reference

## TAPE Checkpoints

The command enforces strict checkpoints:

**✅ Before EXECUTE:**
- [ ] System deeply understood (THINK complete)
- [ ] All evidence gathered (ANALYZE complete)
- [ ] Root cause PROVEN, not guessed (PLAN complete)
- [ ] Multiple hypotheses tested
- [ ] Clear evidence trail documented

**❌ FORBIDDEN:**
- Shotgun debugging (random code changes)
- Trial-and-error (hope-based fixing)
- Fixing symptoms without understanding root cause
- Skipping hypothesis testing

## Output

After completion, you'll receive:

**Debugging Session Report:**
- Original error summary
- Resolution process breakdown
- Technical details of fix
- Quality metrics (resolution time, test results)
- Knowledge captured for future use

## When NOT to Use

Skip `/code/debug` for:
- **Simple syntax errors** - Fix directly
- **Obvious typos** - No TAPE needed
- **Known issues** - Apply known solution
- **Configuration tweaks** - Make change directly

## Philosophy

**"Think twice, code once."**

Every bug deserves systematic analysis. Trial-and-error wastes time and introduces new problems. TAPE ensures you fix the right thing for the right reason.

## Integration with Other Commands

```bash
# After debugging, improve code quality
/code/debug → /code/refactor → /code/dry

# Production workflow
/code/repomix → /code/debug (fix issues) → /code/repomix (verify)

# Complete quality cycle
/code/debug → /code/refactor → /code/dry → /code/repomix
```

## Success Criteria

The debugging workflow succeeds when:
- ✅ Original error completely resolved and verified
- ✅ Fix based on proven root cause (not guessed)
- ✅ All validation tests pass with no regressions
- ✅ System stability confirmed after changes
- ✅ Complete session documentation generated
- ✅ Knowledge captured for similar future issues

## Common Scenarios

### Scenario: Import Error
```bash
/code/debug "ModuleNotFoundError: No module named 'xyz'"

# TAPE Process:
# THINK: Check dependency management, virtual environment
# ANALYZE: Review requirements.txt, check installation
# PLAN: Test hypothesis - missing dependency vs wrong environment
# EXECUTE: Install missing package or activate correct environment
```

### Scenario: Test Failure
```bash
/code/debug "test_user_authentication failing after refactor"

# TAPE Process:
# THINK: Understand test dependencies, what changed in refactor
# ANALYZE: Compare before/after code, check test assumptions
# PLAN: Test theories - API contract changed vs test data stale
# EXECUTE: Update test or fix broken contract
```

### Scenario: Performance Issue
```bash
/code/debug "API response time increased from 100ms to 2000ms"

# TAPE Process:
# THINK: Map request flow, identify components in path
# ANALYZE: Profile code, check database queries, review logs
# PLAN: Test N+1 query vs missing index vs external API slowness
# EXECUTE: Optimize proven bottleneck
```

## Requirements

**Required agents:**
- `error-analysis-specialist` - TAPE Think/Analyze/Plan phases
- `fix-implementation-specialist` - TAPE Execute phase
- `test-analyzer` - Validation and regression testing

**Optional:**
- Language-specific specialists for validation (go-specialist, js-specialist, php-specialist)

## Detailed Workflow

### Phase 1: Repository Context Assessment

**Lightweight reconnaissance:**
- Use Glob to identify relevant files related to error
- Grep for keyword presence checks
- Count occurrences to estimate scope
- Confirm presence/absence of key files mentioned in error

**Approach:**
- Quick file pattern matching (*.go, *.js, etc.)
- Simple keyword searches
- Scope estimation (number of affected files)
- No deep analysis yet (reserved for TAPE phases)

### Phase 2: TAPE Error Analysis

**Delegates to error-analysis-specialist for systematic diagnosis**

**THINK - System Understanding:**
- Map system architecture and component organization
- Understand data flow through affected components
- Identify dependencies and interactions between systems
- Document execution paths and state management

**ANALYZE - Evidence Collection:**
- Gather error symptoms (stack traces, error messages, logs)
- Document reproduction steps (how to trigger the error)
- Examine environment (configuration, versions, system state)
- Review recent changes (git log, recent commits)

**PLAN - Hypothesis Generation:**

Generate 2-3 hypotheses and test systematically:

**Hypothesis 1**: [Most likely root cause]
- Evidence supporting this theory
- Test to prove/disprove
- Result: [Proven/Disproven]

**Hypothesis 2**: [Second possibility]
- Evidence supporting this theory
- Test to prove/disprove
- Result: [Proven/Disproven]

**Hypothesis 3**: [Alternative explanation]
- Evidence supporting this theory
- Test to prove/disprove
- Result: [Proven/Disproven]

**Continue testing until root cause is PROVEN with concrete evidence.**

### TAPE Validation Checkpoint

**Before proceeding to Execute, verify:**
- [ ] System deeply understood (THINK ✓)
- [ ] All evidence gathered (ANALYZE ✓)
- [ ] Root cause PROVEN, not guessed (PLAN ✓)
- [ ] Multiple hypotheses tested systematically
- [ ] Clear evidence trail documented

**IF ANY CHECKBOX UNCHECKED**: Continue Think/Analyze/Plan. DO NOT proceed to Execute.

### Phase 3: TAPE Execute - Fix Implementation

**Delegates to fix-implementation-specialist**

**Implementation steps:**
1. Use Read tool to review affected files
2. Use Edit or MultiEdit to make precise changes
3. Follow existing code patterns and conventions
4. Add error handling where appropriate
5. Verify syntax and logical correctness
6. Test compilation (for compiled languages)

**Quality standards:**
- Maintain code consistency with project patterns
- Implement complete solution, not partial patches
- Add comments explaining non-obvious fixes
- Update related documentation if needed
- Ensure fix doesn't introduce new issues

### Phase 4: Testing and Validation

**Delegates to test-analyzer for comprehensive validation**

**Validation checklist:**
- [ ] Original error resolved
- [ ] No new errors introduced
- [ ] All related tests passing
- [ ] System behaves as expected
- [ ] Performance not degraded
- [ ] Edge cases handled correctly

**Testing commands:**
```bash
# Unit tests
npm test path/to/affected/tests
go test ./path/to/package
pytest tests/test_affected.py

# Integration tests
npm run test:integration

# Manual validation of original error scenario
```

### Phase 5: Knowledge Capture

**For complex debugging sessions**, document patterns for future reference.

**When to document:**
- Complex integration issues
- Novel debugging techniques used
- Multi-system coordination required
- Errors that took >1 hour to diagnose

**Pattern documentation template:**
```markdown
## Debugging Pattern: [Error Type]

### Error Signature
- Symptom: [How the error manifests]
- Common Triggers: [What causes it]
- Affected Systems: [Components involved]

### Diagnosis Approach
1. [Investigation step that worked]
2. [Key insight that led to breakthrough]
3. [Test that proved root cause]

### Root Cause
[Detailed explanation of underlying issue]

### Solution
[Fix implementation with code examples]

### Prevention
- [How to avoid this error in future]
- [Code patterns to prevent recurrence]
- [Monitoring to detect early]
```

## Error Handling & Recovery

**If any phase fails:**
1. Document failure details in TodoWrite with specific error information
2. Attempt retry with refined parameters or additional context
3. If retry fails, escalate to manual intervention with detailed failure report
4. Preserve all intermediate work for human review

**Recovery patterns:**
- **Analysis Failure**: Provide additional context, try different diagnostic approaches
- **Implementation Failure**: Simplify fix approach, break into smaller steps
- **Validation Failure**: Refine fix implementation, address specific test failures

## Tips for Effective Debugging

1. **Provide clear error description**
   - Include stack traces, error messages, logs
   - Describe reproduction steps
   - Mention recent changes (git commits, config updates)

2. **Trust the TAPE process**
   - Don't skip Think/Analyze/Plan to "save time"
   - Systematic investigation is faster than trial-and-error
   - Evidence-based fixes last longer

3. **Validate thoroughly**
   - Test original error scenario
   - Check for regressions in related functionality
   - Verify edge cases work correctly

4. **Capture knowledge**
   - Document complex debugging sessions
   - Help future debugging of similar issues
   - Build organizational debugging expertise
