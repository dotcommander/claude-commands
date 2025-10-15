---
name: learn
description: Context-efficient deep learning extraction using batch processing - identifies genuinely new patterns from documentation recursively
usage: /learn <path> [--output=<path>] [--focus=<topic>]
examples:
  - /learn docs/languages/go
  - /learn docs/frameworks/svelte --output=~/learnings/svelte.md
  - /learn vendor/package/src --focus="concurrency patterns"
notes:
  - Uses batch processing for context efficiency (can handle 1000+ files)
  - Each batch processes 10-15 files and returns insights only
  - Main coordinator synthesizes batch outputs into final document
  - Filters for genuinely new knowledge (not training data synthesis)
---

# Deep Learning Extraction Protocol (Batch Processing)

**Architecture**: You are the **coordinator**. You orchestrate parallel batch processing to extract knowledge, then synthesize the findings.

## Context Efficiency Strategy

**Problem**: Reading all files in main context exhausts 200k token budget quickly.

**Solution**: Parallel batch processing
```
Main Coordinator (you)
├─ Discovers files (Glob tool)
├─ Batches files (10-15 per batch)
├─ Processes batches in parallel
├─ Collects insights (not raw content)
└─ Synthesizes final document

Batch #1 (files 1-15)
└─ Returns: 5-10 insights (500-1000 tokens)

Batch #2 (files 16-30)
└─ Returns: 5-10 insights (500-1000 tokens)

...

Result: Can process 500+ files with <50k tokens
```

## Input Processing

**Target**: `{{arg1}}`
**Output Path**: `{{arg2}}` (default: `{target}/learnings-{timestamp}.md`)
**Focus Area**: `{{arg3}}` (optional: filter insights by topic)

---

## Execution Protocol

### Phase 1: Discovery

**Step 1: Discover all documentation files recursively**

Use Glob tool with multiple patterns in **parallel**:
```
<invoke name="Glob"><path>{{arg1}}</path><pattern>**/*.md</pattern></invoke>
<invoke name="Glob"><path>{{arg1}}</path><pattern>**/*.txt</pattern></invoke>
<invoke name="Glob"><path>{{arg1}}</path><pattern>**/*.rst</pattern></invoke>
<invoke name="Glob"><path>{{arg1}}</path><pattern>**/README*</pattern></invoke>
```

**Step 2: Count and batch files**

```
Total files discovered: {count}

Batching strategy:
- Batch size: 10-15 files per batch (optimal for deep analysis)
- Total batches: ceil({count} / 12)
- If >500 files: Report warning, continue with first 500
```

**Step 3: Report discovery**

```
📁 Target: {{arg1}}
📄 Files discovered: {count}
📦 Batches: {batch_count} (12 files per batch)
⏱️  Estimated time: {batch_count * 2} minutes
```

---

### Phase 2: Batch Processing (Parallel Execution)

**For each batch, process files in parallel with this approach:**

```markdown
**Task**: Extract genuinely new knowledge from these documentation files.

**Files to analyze** (batch #{batch_num}/{total_batches}):
{list of 10-15 file paths}

---

## Novelty Classification Framework (MANDATORY)

Test EVERY insight against this hierarchy. **ONLY include Tier 2-4**.

### Tier 1: Training Data Synthesis ❌ EXCLUDE
**Test**: "Could I have written this without reading the docs?"

**Examples to EXCLUDE**:
- "Use dependency injection for testability" (generic OOP)
- "Validate user input before processing" (basic security)
- "{Framework} has {common feature}" (factual training data)

### Tier 2: Implementation-Specific Details ✅ INCLUDE
**Test**: "Does this show HOW to do something specific to this technology?"

**Examples to INCLUDE**:
- "Laravel's `tap()` enables chaining on void methods by returning the object"
- "Symfony's `#[AsCommand]` auto-registers commands vs manual `services.yaml`"
- "PHP 8.1's `readonly` properties prevent modification after `__construct()`"

### Tier 3: Architectural Decision Insights ✅✅ HIGH VALUE
**Test**: "Does this explain WHY architects made specific trade-offs?"

**Examples to INCLUDE**:
- "Laravel uses facades over DI to reduce service locator complexity, trading testability for DX"
- "Symfony's event system is synchronous by design - use Messenger for async workflows"

### Tier 4: Counter-Intuitive / Corrective ✅✅✅ HIGHEST VALUE
**Test**: "Does this contradict my assumptions or common practice?"

**Examples to INCLUDE**:
- "PSR-4 autoloading is SLOWER than classmap - Laravel uses classmap in production"
- "DB::transaction() does NOT auto-retry on deadlock - manual implementation required"

**Decision Rule**: If uncertain between tiers, EXCLUDE (bias toward high-value novelty).

---

## Your Task

1. **Read all files** in this batch using parallel Read calls
2. **Extract 5-10 genuinely new insights** (Tier 2-4 only)
3. **Return structured JSON** (not prose, not full document)

**Output format** (JSON):

```json
{
  "batch_number": {batch_num},
  "files_analyzed": {count},
  "insights": [
    {
      "tier": 2,
      "pattern_name": "Laravel tap() helper",
      "what_i_learned": "tap() enables method chaining on methods that return void by returning the original object",
      "why_it_matters": "Allows fluent interfaces even when underlying methods don't support chaining",
      "code_example": "return tap($user, fn($u) => $u->save());",
      "source_file": "docs/laravel/helpers.md",
      "source_lines": "145-152",
      "when_to_apply": ["Building fluent APIs", "Chaining operations with side effects"],
      "when_not_to_apply": ["Method already returns $this", "No side effects needed"],
      "confidence": 85
    },
    {
      "tier": 4,
      "pattern_name": "PSR-4 vs Classmap Performance",
      "what_i_learned": "PSR-4 autoloading has filesystem overhead; classmap is 10x faster in production",
      "why_it_matters": "Contradicts assumption that autoloading is 'free' - requires explicit optimization",
      "code_example": "composer dump-autoload -o  # Generates classmap",
      "source_file": "docs/performance/autoloading.md",
      "source_lines": "23-45",
      "anti_pattern": "Using PSR-4 in production without classmap optimization",
      "blast_radius": "Application-wide (every class load)",
      "severity": "high",
      "confidence": 90
    }
  ],
  "focus_filter_applied": {{if arg3}}true{{else}}false{{end}},
  "focus_topic": "{{arg3}}",
  "excluded_count": {count of insights excluded due to Tier 1 classification}
}
```

**Critical Rules**:
1. **ONLY return JSON** (no prose, no explanations outside JSON)
2. **5-10 insights maximum** (quality over quantity)
3. **Copy code examples verbatim** from source (mark if adapted)
4. **Include source attribution** for every insight (file + lines)
5. **Apply focus filter** if `{{arg3}}` is specified (exclude unrelated insights)

---

**Focus Filter Behavior** (if `{{arg3}}` specified):

**INCLUDE**: Insights directly related to `{{arg3}}`
**EXCLUDE**: Unrelated insights (don't summarize just to have content)

Example: If `--focus="concurrency"`:
- ✅ Include: PHP 8.1 Fibers, ReactPHP event loop, Swoole async
- ❌ Exclude: Eloquent ORM, Blade templates, routing patterns

If uncertain whether insight relates to focus, EXCLUDE (bias toward precision).

---

**END OF SUBAGENT PROMPT**
```

**Process all batches in parallel**:

For optimal performance, you can process multiple batches simultaneously using the Task tool if available, or process sequentially if needed. Each batch should:
1. Read the 10-15 assigned files
2. Extract insights using the novelty classification framework
3. Return structured JSON output only
4. Skip raw content to save tokens

**Wait for all batches to complete.**

---

### Phase 3: Synthesis

**Step 1: Collect all batch responses**

Parse JSON from each batch:
- Total insights: {sum of all insights}
- Tier 2 (Implementation): {count}
- Tier 3 (Architectural): {count}
- Tier 4 (Counter-intuitive): {count}
- Excluded (Tier 1): {count}

**Step 2: Deduplicate insights**

If multiple batches found same pattern:
- Keep highest confidence version
- Merge "when to apply" scenarios
- Note pattern appeared in multiple files

**Step 3: Sort by tier and confidence**

Priority order:
1. Tier 4 insights (counter-intuitive) - highest value
2. Tier 3 insights (architectural) - high value
3. Tier 2 insights (implementation) - standard value

Within each tier, sort by confidence (high → low).

**Step 4: Generate final learning document**

Use template below with collected insights.

---

## Learning Document Template

```markdown
# Deep Learning: {{arg1 basename}} - {{date}}

**Source**: `{{arg1}}`
{{if arg3}}**Focus**: {{arg3}}{{end}}
**Files Analyzed**: {total_files}
**Batches Processed**: {batch_count}
**Insights Extracted**: {total_insights}
**Session Duration**: {minutes}

---

## 🎯 Quick Navigation

- [🆕 Genuinely New Knowledge](#-genuinely-new-knowledge) ({tier_2_count} + {tier_3_count} insights)
- [💥 Counter-Intuitive Patterns](#-counter-intuitive-patterns) ({tier_4_count} insights)
- [🚫 Anti-Patterns](#-anti-patterns) ({anti_pattern_count})
- [⚡ Immediate Actions](#-immediate-actions) ({action_count})
- [📊 Knowledge Delta](#-knowledge-delta)

---

## 💥 Counter-Intuitive Patterns (Tier 4)

{{for each tier_4_insight}}
### {pattern_name}

**What I Learned**: {what_i_learned}

**Why It Matters**: {why_it_matters}

**Contradicts**: {common assumption this contradicts}

**Code Example**:
```{language}
{code_example}
```
**Source**: `{source_file}:{source_lines}`

{{if anti_pattern}}
**Anti-Pattern** (Severity: {severity}):
```{language}
{anti_pattern}
```
**Blast Radius**: {blast_radius}
{{end}}

**When to Apply**:
{{for each when_to_apply}}
- {scenario}
{{end}}

**When NOT to Apply**:
{{for each when_not_to_apply}}
- {scenario}
{{end}}

**Confidence**: {confidence}%

---
{{end}}

## 🏗️ Architectural Insights (Tier 3)

{{for each tier_3_insight}}
### {pattern_name}

**What I Learned**: {what_i_learned}

**Design Philosophy**: {why architects made this choice}

**Trade-offs**:
- ✅ **Pros**: {benefits}
- ❌ **Cons**: {costs}

**Code Example**:
```{language}
{code_example}
```
**Source**: `{source_file}:{source_lines}`

**Decision Criteria**: {when to use this approach}

**Confidence**: {confidence}%

---
{{end}}

## 🔧 Implementation Patterns (Tier 2)

{{for each tier_2_insight}}
### {pattern_name}

**What I Learned**: {what_i_learned}

**Code Example**:
```{language}
{code_example}
```
**Source**: `{source_file}:{source_lines}`

**When to Apply**:
{{for each when_to_apply}}
- {scenario}
{{end}}

**Confidence**: {confidence}%

---
{{end}}

## 🚫 Anti-Patterns

{{for each anti_pattern_insight}}
### {pattern_name}

**Severity**: {severity} (Blast Radius: {blast_radius})

**What NOT to Do**:
```{language}
{anti_pattern}
```

**Why It's Dangerous**: {specific risks}

**Correct Approach**:
```{language}
{code_example}
```

**Source**: `{source_file}:{source_lines}`

---
{{end}}

## ⚡ Immediate Actions

**Prioritized by tier and confidence:**

{{for each insight, sorted by tier DESC, confidence DESC}}
### {pattern_name}

**Priority**: {tier == 4 ? "🔴 Critical" : tier == 3 ? "🟠 High" : "🟡 Medium"}

**Concrete Action**:

{{if applicable_to_current_project}}
**Code Refactoring**:
- **Target**: {suggest file/module where this could be applied}
- **Change**: {specific refactoring}
- **Validation**: {how to verify it works}
- **Timeline**: This week
{{else}}
**Mental Model Update**:
- **Old assumption**: {common misconception}
- **New understanding**: {what_i_learned}
- **Remember when**: {future scenario where this applies}
- **Timeline**: Internalized immediately
{{end}}

---
{{end}}

## 📊 Knowledge Delta

**Insights by Tier**:
- 💥 Tier 4 (Counter-intuitive): {tier_4_count}
- 🏗️  Tier 3 (Architectural): {tier_3_count}
- 🔧 Tier 2 (Implementation): {tier_2_count}
- ❌ Tier 1 (Excluded): {excluded_count}

**Confidence Distribution**:
- High (80-100%): {high_confidence_count}
- Medium (60-79%): {medium_confidence_count}
- Low (40-59%): {low_confidence_count}

**Learning Efficiency**:
- Files per insight: {files_analyzed / insights_count}
- Insights per batch: {avg_insights_per_batch}
- Quality ratio: {(tier_3 + tier_4) / total_insights}%

{{if focus_topic}}
**Focus Discipline**: {focus_filtered_count}/{total_potential_insights} insights matched focus "{focus_topic}" ({percentage}%)
{{end}}

**Validation Plan**:
{{for top 3 tier_4 insights}}
1. {pattern_name}: {how to validate in practice}
{{end}}

---

## 📚 Sources Analyzed

{{for each file analyzed}}
- `{file_path}` ({insights_count} insights)
{{end}}

---

**Learning Session Complete**: {{timestamp}}

**Recommended Next Steps**:
1. Apply Tier 4 insights immediately (highest ROI)
2. Validate counter-intuitive patterns with benchmarks/tests
3. Update team documentation with architectural insights
{{if focus_topic}}4. Run `/learn {{arg1}} --focus="{complementary_topic}"` for related areas{{end}}

```

---

## Final Output (Main Coordinator)

After generating the learning document:

```
🧠 Deep Learning Session Complete

📁 Source: {{arg1}}
📄 Files Analyzed: {count}
📦 Batches: {batch_count}
🆕 Total Insights: {count}
├─ 💥 Tier 4 (Counter-intuitive): {count}
├─ 🏗️  Tier 3 (Architectural): {count}
└─ 🔧 Tier 2 (Implementation): {count}

❌ Excluded (Tier 1 - training data): {count}
{{if focus_topic}}🎯 Focus filtered ("{focus_topic}"): {matched}/{total}{{end}}

⏱️  Total Time: {minutes}
📝 Document: {output_path}

💡 Top 3 Insights (highest value):
1. [{tier}] {pattern_name} - {one_line_summary}
2. [{tier}] {pattern_name} - {one_line_summary}
3. [{tier}] {pattern_name} - {one_line_summary}

🎯 Recommended Actions:
- {Action 1 from highest priority insight}
- {Action 2 from second highest priority insight}
- {Action 3 from third highest priority insight}
```

---

## Context Efficiency Metrics

**Current implementation**:
- Main coordinator: <10k tokens (discovery + synthesis)
- Per subagent: 20-30k tokens (read 12 files, extract insights)
- Subagent response: 500-1000 tokens (JSON only, no raw content)
- **Total for 500 files**: ~50k tokens (fits comfortably in budget)

**Comparison to direct reading**:
- Direct: 500 files × 5k avg = 2,500k tokens ❌ (exceeds budget)
- Subagent: 42 batches × 1k response = ~50k tokens ✅ (10x savings)

---

## Edge Cases & Guardrails

**Scenario: >500 files discovered**
```
⚠️  Large codebase detected (1,234 files).

Analyzing first 500 files (42 batches)...

**Recommendation**: Use focused approach for remaining files:
- /learn {{arg1}}/subdirectory --focus="specific-topic"
```

**Scenario: Batch returns Tier 1 only**
```
⚠️  Batch {num}: 0 insights extracted (all Tier 1 - training data synthesis).

This may indicate:
- Documentation is too basic/generic
- Focus topic doesn't match content
- Filtering too strict

Continuing with remaining batches...
```

**Scenario: Context approaching limit**
```
⚠️  Context usage: 180k/200k tokens (90%).

Stopping after batch {num}/{total}.

**Analyzed**: {files_analyzed}/{total_files} files
**Recommendation**: Run again with `--focus` to analyze remaining files.
```

---

**Philosophy**: Transform documentation reading from context-exhausting raw ingestion to efficient insight extraction through parallel batch processing. Quality over quantity - 10 genuinely new insights beats 100 training-data rehashes.
