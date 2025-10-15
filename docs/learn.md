# /learn - Deep Learning Extraction

**Extract genuinely novel insights from large documentation sets**

## Overview

The `/learn` command processes large documentation sets (1000+ files) efficiently by filtering out "training data synthesis" to surface only counter-intuitive patterns and architectural insights you wouldn't discover otherwise. Uses a subagent architecture for context efficiency.

## When to Use

Use `/learn` when:
- **Learning new framework** - Extract key patterns from docs
- **Studying vendor code** - Understand design decisions
- **Onboarding to codebase** - Find non-obvious patterns
- **Evaluating technology** - Identify architectural trade-offs
- **Documentation audit** - Extract actionable insights
- **Before major refactoring** - Understand current patterns
- **After dependency update** - Learn new features and changes

## Usage Examples

```bash
# Learn from Go documentation
/learn docs/languages/go

# Focus on specific topic
/learn vendor/package/src --focus="concurrency patterns"

# Custom output location
/learn docs/frameworks/svelte --output=~/learnings/svelte.md

# Large documentation set
/learn node_modules/@types/react --focus="hooks"
```

## How It Works

### Subagent Architecture

Instead of reading all files in main context (exhausts tokens quickly), `/learn` uses parallel subagents:

```
Main Coordinator
├─ Discovers all .md/.txt/.rst files
├─ Batches files (10-15 per subagent)
├─ Dispatches subagents in parallel
├─ Collects insights (not raw content)
└─ Synthesizes final document

Each Subagent
├─ Reads 10-15 files
├─ Extracts 5-10 insights
└─ Returns structured JSON (500-1000 tokens)

Result: Process 500+ files with <50k tokens (10x efficiency)
```

### Novelty Classification

All insights are classified into 4 tiers. **Only Tier 2-4 included:**

**Tier 1: Training Data Synthesis** ❌ **EXCLUDED**
- Generic advice you already know
- "Use dependency injection for testability"
- "Validate user input before processing"
- Basic framework features

**Tier 2: Implementation-Specific Details** ✅ **INCLUDED**
- Technology-specific HOW-TO patterns
- "Laravel's `tap()` enables chaining on void methods"
- "Symfony's `#[AsCommand]` auto-registers commands"
- Concrete examples with code

**Tier 3: Architectural Decision Insights** ✅✅ **HIGH VALUE**
- WHY architects made specific trade-offs
- Design philosophy and decision rationale
- "Laravel uses facades over DI to reduce service locator complexity"
- "Symfony's event system is synchronous by design - use Messenger for async"

**Tier 4: Counter-Intuitive / Corrective** ✅✅✅ **HIGHEST VALUE**
- Contradicts common assumptions
- Corrects widespread misconceptions
- "PSR-4 autoloading is SLOWER than classmap - Laravel uses classmap in production"
- "DB::transaction() does NOT auto-retry on deadlock"

### Focus Filter

When `--focus` is specified:
- **ONLY include** insights directly related to focus topic
- **Exclude** unrelated insights (even if high-tier)
- Bias toward precision over completeness

Example with `--focus="concurrency"`:
- ✅ Include: PHP 8.1 Fibers, ReactPHP event loop, async patterns
- ❌ Exclude: Eloquent ORM, routing, Blade templates

## Output Structure

The learning document contains:

### Counter-Intuitive Patterns (Tier 4)
Highest value - contradicts assumptions:
- Pattern name and explanation
- Why it matters
- What common assumption it contradicts
- Code examples
- Anti-patterns to avoid
- When to apply / when not to apply

### Architectural Insights (Tier 3)
Design philosophy and trade-offs:
- Pattern explanation
- Design rationale
- Trade-offs (pros/cons)
- Decision criteria
- Code examples

### Implementation Patterns (Tier 2)
Technology-specific techniques:
- Pattern explanation
- Code examples
- When to apply
- Source file references

### Anti-Patterns
What NOT to do:
- Severity and blast radius
- Dangerous pattern examples
- Why it's problematic
- Correct approach

### Immediate Actions
Prioritized by tier and confidence:
- Code refactoring recommendations
- Mental model updates
- Validation plans

### Knowledge Delta
Learning efficiency metrics:
- Insights by tier distribution
- Confidence distribution
- Files per insight ratio
- Quality ratio (Tier 3+4 / total)

## What to Expect

After running `/learn`:

1. **Discovery phase**
   ```
   📁 Target: vendor/react/docs
   📄 Files discovered: 237
   📦 Batches: 20 (12 files per batch)
   ⏱️  Estimated time: 40 minutes
   ```

2. **Parallel processing**
   - All subagents run simultaneously
   - Each extracts 5-10 insights
   - Returns only high-value findings

3. **Synthesis**
   ```
   🧠 Deep Learning Session Complete

   📄 Files Analyzed: 237
   🆕 Total Insights: 43
   ├─ 💥 Tier 4 (Counter-intuitive): 8
   ├─ 🏗️  Tier 3 (Architectural): 15
   └─ 🔧 Tier 2 (Implementation): 20

   ❌ Excluded (Tier 1 - training data): 89
   ```

4. **Learning document**
   - Saved to `learnings-{timestamp}.md`
   - Organized by value tier
   - Code examples included
   - Source file references

## Key Benefits

**Context Efficiency:**
- Direct reading: 500 files × 5k tokens = 2,500k tokens ❌ (exceeds budget)
- Subagent approach: 42 batches × 1k response = ~50k tokens ✅ (10x savings)

**Quality Over Quantity:**
- Filters out generic advice
- Surfaces counter-intuitive patterns
- Focuses on actionable insights

**Systematic Coverage:**
- Processes entire documentation sets
- Doesn't miss hidden gems
- Consistent quality across all files

## When NOT to Use

Skip `/learn` for:
- **Simple tutorials** - Just read directly
- **Single file** - Use Read tool instead
- **Well-known frameworks** - Training data already covered
- **Time-critical learning** - Too thorough for quick lookups

## Philosophy

**"Transform documentation reading from context-exhausting raw ingestion to efficient insight extraction."**

Quality over quantity: 10 genuinely new insights beats 100 training-data rehashes.

## Integration with Other Commands

```bash
# Learn framework patterns, then apply
/learn vendor/framework/docs → Review insights → /code/refactor

# Documentation workflow
/learn docs/ → Extract patterns → /content/write (document findings)

# Pre-refactoring research
/learn vendor/package/src → Understand patterns → /code/refactor

# Technology evaluation
/learn candidate-framework/docs → Compare insights → Make decision
```

## Success Criteria

Learning succeeds when:
- ✅ High percentage of Tier 3-4 insights (>40%)
- ✅ Counter-intuitive patterns identified
- ✅ Actionable recommendations generated
- ✅ Code examples included for all patterns
- ✅ Source files properly referenced
- ✅ Focus filter applied correctly (if specified)

## Common Scenarios

### Scenario: New Framework Onboarding
```bash
/learn vendor/new-framework/docs --focus="best practices"

# Review Tier 4 insights first
# Apply counter-intuitive patterns immediately
# Validate with tests
```

### Scenario: Vendor Code Audit
```bash
/learn vendor/package/src

# Understand design decisions
# Identify patterns to replicate
# Spot anti-patterns to avoid
```

### Scenario: Pre-Refactoring Research
```bash
# Before refactoring authentication:
/learn src/auth --focus="security patterns"

# Extract current patterns
# Identify improvements
# Plan refactoring based on insights
```

### Scenario: Documentation Consolidation
```bash
# Learn from scattered docs:
/learn docs/

# Extract patterns
# Identify duplicate content
# Use insights for /content/dedup
```

## Edge Cases

**Large codebase (>500 files):**
```
⚠️  Large codebase detected (1,234 files).

Analyzing first 500 files (42 batches)...

Recommendation: Use focused approach for remaining:
- /learn src/subdirectory --focus="specific-topic"
```

**No insights found:**
```
⚠️  Batch 5: 0 insights extracted (all Tier 1).

This may indicate:
- Documentation is too basic/generic
- Focus topic doesn't match content
- Subagent filtering too strict
```

**Context approaching limit:**
```
⚠️  Context usage: 180k/200k tokens (90%).

Stopping after batch 35/42.

Analyzed: 420/504 files
Recommendation: Run again with --focus for remaining.
```
