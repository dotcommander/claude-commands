# Learning Document Template

Use this template when synthesizing batch results into the final output file.

---

## Template

```markdown
# Deep Learning: {source_basename} - {date}

**Source**: `{target}`
{if focus_topic}**Focus**: {focus_topic}{end}
**Files Analyzed**: {total_files}
**Batches Processed**: {batch_count}
**Insights Extracted**: {total_insights}
**Session Duration**: {minutes}

---

## Quick Navigation

- [Counter-Intuitive Patterns](#counter-intuitive-patterns-tier-4) ({tier_4_count} insights)
- [Architectural Insights](#architectural-insights-tier-3) ({tier_3_count} insights)
- [Implementation Patterns](#implementation-patterns-tier-2) ({tier_2_count} insights)
- [Anti-Patterns](#anti-patterns) ({anti_pattern_count})
- [Immediate Actions](#immediate-actions) ({action_count})
- [Knowledge Delta](#knowledge-delta)

---

## Counter-Intuitive Patterns (Tier 4)

{for each tier_4_insight}
### {pattern_name}

**What I Learned**: {what_i_learned}

**Why It Matters**: {why_it_matters}

**Contradicts**: {common assumption this contradicts}

**Code Example**:
```{language}
{code_example}
```

**Source**: `{source_file}:{source_lines}`

{if anti_pattern}
**Anti-Pattern** (Severity: {severity}):
```{language}
{anti_pattern}
```
**Blast Radius**: {blast_radius}
{end}

**When to Apply**:
{for each when_to_apply}
- {scenario}
{end}

**When NOT to Apply**:
{for each when_not_to_apply}
- {scenario}
{end}

**Confidence**: {confidence}%

---
{end}

## Architectural Insights (Tier 3)

{for each tier_3_insight}
### {pattern_name}

**What I Learned**: {what_i_learned}

**Design Philosophy**: {why architects made this choice}

**Trade-offs**:
- Pros: {benefits}
- Cons: {costs}

**Code Example**:
```{language}
{code_example}
```

**Source**: `{source_file}:{source_lines}`

**Decision Criteria**: {when to use this approach}

**Confidence**: {confidence}%

---
{end}

## Implementation Patterns (Tier 2)

{for each tier_2_insight}
### {pattern_name}

**What I Learned**: {what_i_learned}

**Code Example**:
```{language}
{code_example}
```

**Source**: `{source_file}:{source_lines}`

**When to Apply**:
{for each when_to_apply}
- {scenario}
{end}

**Confidence**: {confidence}%

---
{end}

## Anti-Patterns

{for each anti_pattern_insight}
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
{end}

## Immediate Actions

**Prioritized by tier and confidence:**

{for each insight, sorted by tier DESC, confidence DESC}
### {pattern_name}

**Priority**: {tier == 4 ? "Critical" : tier == 3 ? "High" : "Medium"}

**Concrete Action**:

{if applicable_to_current_project}
**Code Refactoring**:
- Target: {suggest file/module where this could be applied}
- Change: {specific refactoring}
- Validation: {how to verify it works}
- Timeline: This week
{else}
**Mental Model Update**:
- Old assumption: {common misconception}
- New understanding: {what_i_learned}
- Remember when: {future scenario where this applies}
- Timeline: Internalized immediately
{end}

---
{end}

## Knowledge Delta

**Insights by Tier**:
- Tier 4 (Counter-intuitive): {tier_4_count}
- Tier 3 (Architectural): {tier_3_count}
- Tier 2 (Implementation): {tier_2_count}
- Tier 1 (Excluded): {excluded_count}

**Confidence Distribution**:
- High (80-100%): {high_confidence_count}
- Medium (60-79%): {medium_confidence_count}
- Low (40-59%): {low_confidence_count}

**Learning Efficiency**:
- Files per insight: {files_analyzed / insights_count}
- Insights per batch: {avg_insights_per_batch}
- Quality ratio: {(tier_3 + tier_4) / total_insights}%

{if focus_topic}
**Focus Discipline**: {focus_filtered_count}/{total_potential_insights} insights matched focus "{focus_topic}" ({percentage}%)
{end}

**Validation Plan**:
{for top 3 tier_4 insights}
1. {pattern_name}: {how to validate in practice}
{end}

---

## Sources Analyzed

{for each file analyzed}
- `{file_path}` ({insights_count} insights)
{end}

---

**Learning Session Complete**: {timestamp}

**Recommended Next Steps**:
1. Apply Tier 4 insights immediately (highest ROI)
2. Validate counter-intuitive patterns with benchmarks/tests
3. Update team documentation with architectural insights
{if focus_topic}4. Run `/learn {target} --focus="{complementary_topic}"` for related areas{end}
```
