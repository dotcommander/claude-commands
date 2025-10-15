---
description: Systematic debugging workflow using TAPE methodology (Think, Analyze, Plan, Execute)
argument-hint: <error-description-or-empty>
---

# /code/debug - Systematic Debugging Workflow

## 🚨 SUPREME DIRECTIVE: TAPE Methodology Enforcement
**You MUST ALWAYS fix issues using the TAPE method. Think. Analyze. Plan. Execute. In that order!**

Execute systematic TAPE-based debugging workflow for error: $ARGUMENTS

**NO trial-and-error. NO shotgun debugging. ONLY methodical problem-solving.**

## TodoWrite Phase Tracking (MANDATORY)

Initialize TodoWrite with TAPE-based debugging workflow phases:
1. ☐ Repository context assessment
2. ☐ THINK Phase: System understanding and data flow mapping
3. ☐ ANALYZE Phase: Error diagnosis and evidence gathering
4. ☐ PLAN Phase: Hypothesis generation and systematic testing
5. ☐ TAPE Validation: Verify all phases complete before Execute
6. ☐ EXECUTE Phase: Fix implementation with proven root cause
7. ☐ EXECUTE Phase: Testing and validation of implemented fixes
8. ☐ Session documentation and knowledge capture

## Phase 1: Repository Context Assessment

**ACTION**: Perform lightweight repository reconnaissance to scope the debugging issue

**RECONNAISSANCE SCOPE**:
- Use Glob to identify relevant files related to error (if file/module mentioned)
- Use Grep for lightweight keyword presence checks
- Count occurrences to estimate scope
- Confirm presence/absence of key files mentioned in error

**APPROACH**:
- Quick file pattern matching
- Simple keyword searches
- Scope estimation
- No deep analysis yet (save for Phase 2)

**PERMITTED QUERIES**:
```bash
# Example reconnaissance
- Glob pattern matching: *.go, *.js, etc.
- Simple keyword presence: grep -l "ErrorSymbol" **/*.go
- Scope estimation: grep -c "function_name" **/*.js
```

**SUCCESS**: Basic context gathered, relevant files identified for analysis
**NEXT**: Mark Phase 1 complete, proceed to Phase 2

## Phase 2: TAPE Think/Analyze/Plan Phases - Error Analysis

**🚨 TAPE CHECKPOINT: Think → Analyze → Plan (No Execute Yet!)**

**ACTION**: Perform systematic error diagnosis using TAPE methodology

### THINK Phase

**Understand the system:**

Use Read and Grep tools to understand:
- System architecture and component organization
- Data flow through the affected components
- Dependencies and interactions
- Execution paths and state management

```bash
# Example investigation commands
grep -r "function_name" src/
grep -r "class ClassName" src/
find . -name "*service*" -o -name "*handler*"
```

**Document your understanding:**
- How does this component fit in the system?
- What are its dependencies?
- How does data flow through it?

### ANALYZE Phase

**Gather all evidence:**

- **Error symptoms**: Stack traces, error messages, logs
- **Reproduction steps**: How to trigger the error
- **Environment**: Configuration, versions, system state
- **Recent changes**: Git log, recent commits

```bash
# Gather evidence
git log --oneline --since="1 week ago" -- path/to/affected/files
git diff HEAD~5..HEAD path/to/affected/files
```

**Document findings:**
- What exactly happens when the error occurs?
- When did it start happening?
- What changed recently?

### PLAN Phase

**Generate and test hypotheses:**

1. **Hypothesis 1**: [Most likely root cause]
   - Evidence supporting this theory
   - Test to prove/disprove
   - Result: [Proven/Disproven]

2. **Hypothesis 2**: [Second possibility]
   - Evidence supporting this theory
   - Test to prove/disprove
   - Result: [Proven/Disproven]

3. **Hypothesis 3**: [Alternative explanation]
   - Evidence supporting this theory
   - Test to prove/disprove
   - Result: [Proven/Disproven]

**Systematically test each hypothesis** until root cause is PROVEN.

### TAPE Validation Checkpoint

Before proceeding to EXECUTE, verify:
- [ ] System deeply understood (THINK ✓)
- [ ] All evidence gathered (ANALYZE ✓)
- [ ] Root cause PROVEN, not guessed (PLAN ✓)
- [ ] Multiple hypotheses tested systematically
- [ ] Clear evidence trail documented

**IF ANY CHECKBOX UNCHECKED**: Continue Think/Analyze/Plan. DO NOT proceed to Execute.

**SUCCESS CRITERIA**: Root cause identified with concrete evidence and tested hypotheses
**NEXT**: Mark Phase 2 complete, proceed to Phase 3

## Phase 3: TAPE Execute Phase - Fix Implementation

**🚨 TAPE VALIDATION GATE: Verify Think/Analyze/Plan Complete**

**MANDATORY PRE-EXECUTION CHECKS**:

Before ANY code changes, confirm:
- [ ] System understanding documented (THINK ✓)
- [ ] Evidence-based root cause (ANALYZE ✓)
- [ ] Tested hypotheses with proof (PLAN ✓)
- [ ] NO assumptions or guesses
- [ ] Clear fix strategy validated

**IF ANY CHECKBOX UNCHECKED**: Return to Think/Analyze/Plan phases. DO NOT write code.

### ACTION: Implement the Proven Fix

Based on validated TAPE analysis, implement the fix:

**Fix Implementation Steps:**

1. **Use Read tool** to review affected files
2. **Use Edit or MultiEdit** to make precise changes
3. **Follow existing code patterns** and conventions
4. **Add error handling** where appropriate
5. **Verify syntax** and logical correctness
6. **Test compilation** (for compiled languages)

**Quality Standards:**
- Maintain code consistency with project patterns
- Implement complete solution, not partial patches
- Add comments explaining non-obvious fixes
- Update related documentation if needed
- Ensure fix doesn't introduce new issues

**Example fix workflow:**
```bash
# Read the file to understand current state
Read: src/affected-file.js

# Apply the fix using Edit tool
Edit: src/affected-file.js
- old_string: [problematic code]
- new_string: [corrected code based on proven root cause]

# Verify the fix compiles/runs
Bash: npm run build  # or appropriate build/test command
```

**SUCCESS CRITERIA**: Fix implemented with verification of syntax/logic correctness
**NEXT**: Mark Phase 3 complete, proceed to Phase 4

## Phase 4: Testing and Validation

**ACTION**: Validate fix implementation through systematic testing

### Validation Steps

1. **Run relevant tests** to verify fix works
2. **Check for regressions** in related functionality
3. **Validate original error** no longer occurs
4. **Test edge cases** related to the fix
5. **Verify system stability** after changes

**Testing Scope:**

```bash
# Unit tests for modified components
npm test path/to/affected/tests
# or
go test ./path/to/package
# or
pytest tests/test_affected.py
# or
phpunit tests/AffectedTest.php

# Integration tests if system interactions changed
npm run test:integration
# or appropriate integration test command

# Manual validation of error scenario
# Reproduce original error conditions and verify it's fixed
```

**Validation Checklist:**
- [ ] Original error resolved
- [ ] No new errors introduced
- [ ] All related tests passing
- [ ] System behaves as expected
- [ ] Performance not degraded
- [ ] Edge cases handled correctly

**Document Results:**

```markdown
## Validation Results

**Original Error**: [Description]
**Status**: ✅ RESOLVED / ❌ STILL FAILING

**Tests Run**:
- Unit tests: X passed, Y failed
- Integration tests: X passed, Y failed
- Manual validation: [PASS/FAIL]

**Regressions Detected**: [None / List any new issues]

**Performance Impact**: [None / Description]

**Conclusion**: Fix is [COMPLETE / NEEDS REFINEMENT]
```

**SUCCESS CRITERIA**: Fix validated with no regressions, original error resolved
**FAILURE**: Tests fail or regressions detected → Return to Phase 3 for fix refinement
**NEXT**: Mark Phase 4 complete, proceed to Phase 5

## Phase 5: Knowledge Capture (Conditional)

**ACTION**: For complex debugging sessions, document patterns for future reference

**COMPLEXITY ASSESSMENT**:
- Simple syntax errors: Skip pattern extraction
- Complex integration issues: Document patterns
- Novel debugging techniques used: Document patterns
- Multi-system coordination: Document patterns

**IF DOCUMENTATION WARRANTED**, create debugging pattern document:

```markdown
## Debugging Pattern: [Error Type]

### Error Signature
- **Symptom**: [How the error manifests]
- **Common Triggers**: [What causes it]
- **Affected Systems**: [Components involved]

### Diagnosis Approach
1. [Investigation step 1]
2. [Investigation step 2]
3. [Key insight that led to root cause]

### Root Cause
[Detailed explanation of the underlying issue]

### Solution
[Fix implementation with code examples]

### Prevention
- [How to avoid this error in future]
- [Code patterns to prevent recurrence]
- [Monitoring to detect early]

### Similar Issues
- [Related errors that may have same root cause]
- [Variations of this pattern]
```

**SUCCESS CRITERIA**: Debugging patterns documented for future reference
**SKIP CONDITIONS**: Simple errors or well-known patterns
**NEXT**: Mark Phase 5 complete, proceed to Phase 6

## Phase 6: Knowledge Capture and Session Documentation

**ACTION**: Generate comprehensive debugging session report

Create detailed session summary:

```markdown
# Debugging Session Report
**Date**: [CURRENT_TIMESTAMP]
**Original Error**: $ARGUMENTS
**Session Duration**: [DURATION]
**Complexity Level**: [1-20 scale]

## Error Summary
- **Type**: [Error classification]
- **Severity**: [Impact level]
- **Root Cause**: [Specific cause identified]
- **Affected Systems**: [Components involved]

## Resolution Process
| Phase | Action | Duration | Status |
|-------|--------|----------|--------|
| 1 | Context Assessment | [TIME] | ✅ |
| 2 | THINK: System Understanding | [TIME] | ✅ |
| 3 | ANALYZE: Evidence Gathering | [TIME] | ✅ |
| 4 | PLAN: Hypothesis Testing | [TIME] | ✅ |
| 5 | EXECUTE: Fix Implementation | [TIME] | ✅ |
| 6 | Testing & Validation | [TIME] | ✅ |
| 7 | Knowledge Capture | [TIME] | [STATUS] |

## Technical Details
**Files Modified**: [List of changed files]
**Fix Summary**: [Brief description of solution]
**Tests Executed**: [Test validation summary]

## Quality Metrics
- **Resolution Time**: [TOTAL_DURATION]
- **Fix Verification**: [PASS/FAIL]
- **Regression Testing**: [PASS/FAIL]
- **Pattern Extracted**: [YES/NO]

## Knowledge Captured
- **Debugging Patterns**: [If extracted, reference to memory storage]
- **Reusable Techniques**: [Key approaches for similar issues]
- **Prevention Strategies**: [How to avoid similar errors]

## Success Validation
✅ Original error completely resolved
✅ Fix implemented and validated
✅ No regressions introduced
✅ Knowledge captured for future reference
```

**SUCCESS CRITERIA**: Complete session documentation with all metrics
**NEXT**: Mark Phase 6 complete, workflow COMPLETE

## Error Handling and Recovery

### Phase Failure Recovery
If any phase fails:
1. Document failure details in TodoWrite with specific error information
2. Attempt retry with refined parameters or additional context
3. If retry fails, escalate to manual intervention with detailed failure report
4. Partial solutions should be documented even if complete resolution fails

### Workflow Recovery Patterns
**Analysis Failure**: Provide additional context, try different diagnostic approaches
**Implementation Failure**: Simplify fix approach, break into smaller steps
**Validation Failure**: Refine fix implementation, address specific test failures

### Emergency Escalation
When automated workflow cannot resolve:
1. Document complete debugging session state
2. Provide all analysis results and attempted fixes
3. Generate manual debugging guide with next steps
4. Preserve all intermediate work for human review

## Success Criteria

The debugging workflow succeeds when:
- ✅ Original error completely resolved and verified
- ✅ Fix implemented following systematic analysis
- ✅ All validation tests pass with no regressions
- ✅ Complete session documentation generated
- ✅ Knowledge captured for future similar issues
- ✅ System stability confirmed after changes

## Command Usage Examples

```bash
# Debug with specific error message
/debug "ImportError: module 'requests' not found"

# Debug system-wide issue
/debug "Server returns 500 errors intermittently"

# Debug with context
/debug "Tests failing after dependency update"

# General debugging session
/debug
```

## Workflow Integration

This debug orchestrator integrates with:
- **Error monitoring systems**: Process alerts and error reports
- **CI/CD pipelines**: Handle build and deployment failures
- **Development workflow**: Systematic issue resolution
- **Knowledge management**: Pattern extraction and reuse

---

**PHILOSOPHY**: Systematic debugging through TAPE methodology. Each phase builds on the previous, ensuring thorough analysis, proper implementation, and complete validation. Think twice, code once. Every bug deserves methodical investigation.