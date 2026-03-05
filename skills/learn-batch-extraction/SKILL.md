---
name: learn-batch-extraction
description: |
  Novelty classification framework for extracting genuinely new knowledge from documentation.
  Use when: processing docs with /learn, extracting insights from large codebases, batch knowledge extraction.
  NOT for: summarizing documentation, generating training data, producing general overviews.
---

# Batch Learning Extraction

Extracts Tier 2-4 insights from documentation via parallel batch processing. Filters out training data synthesis (Tier 1).

## Quick Reference

| Intent | Action |
|--------|--------|
| "Extract insights from docs" | Apply Tier 1-4 classification, return JSON |
| "What tier is this insight?" | Use decision tests below |
| "Build learning document" | Read(references/document-template.md) |
| "Handle >500 files" | Warn, cap at 500, suggest --focus |
| "All insights in batch are Tier 1" | Return empty insights array, report excluded_count |

**MUST (BLOCKING):**
- ONLY return Tier 2-4 insights — never include Tier 1
- Return JSON only from batch subagents — no prose, no explanations
- Include source attribution (file + lines) for every insight
- When uncertain between tiers, EXCLUDE (bias toward high-value novelty)

## Workflow

1. **Classify** — Apply Novelty Classification tiers (below) to each insight candidate
2. **Format** — Return insights as Batch JSON Schema (below) — no prose
3. **Filter** — Apply focus filter if `--focus` specified, exclude unrelated
4. **Deduplicate** — Merge same pattern_name across batches, keep highest confidence
5. **Sort** — Tier 4 first, then Tier 3, then Tier 2; within each tier by confidence descending
6. **Write** — Generate final document using `Read(references/document-template.md)` template

## Novelty Classification

### Tier 1: Training Data Synthesis — EXCLUDE

**Test**: "Could I have written this without reading the docs?"

Exclude patterns like:
- "Use dependency injection for testability" (generic OOP)
- "Validate user input before processing" (basic security)
- "{Framework} has {common feature}" (factual training data)

### Tier 2: Implementation-Specific Details — INCLUDE

**Test**: "Does this show HOW to do something specific to this technology?"

Include patterns like:
- "Laravel's `tap()` enables chaining on void methods by returning the object"
- "Symfony's `#[AsCommand]` auto-registers commands vs manual `services.yaml`"
- "PHP 8.1's `readonly` properties prevent modification after `__construct()`"

### Tier 3: Architectural Decision Insights — HIGH VALUE

**Test**: "Does this explain WHY architects made specific trade-offs?"

Include patterns like:
- "Laravel uses facades over DI to reduce service locator complexity, trading testability for DX"
- "Symfony's event system is synchronous by design — use Messenger for async workflows"

### Tier 4: Counter-Intuitive / Corrective — HIGHEST VALUE

**Test**: "Does this contradict assumptions or common practice?"

Include patterns like:
- "PSR-4 autoloading is SLOWER than classmap — Laravel uses classmap in production"
- "DB::transaction() does NOT auto-retry on deadlock — manual implementation required"

## Batch JSON Schema

Each batch subagent returns this structure (no prose):

```json
{
  "batch_number": 1,
  "files_analyzed": 12,
  "insights": [
    {
      "tier": 2,
      "pattern_name": "Laravel tap() helper",
      "what_i_learned": "tap() enables method chaining on methods that return void",
      "why_it_matters": "Allows fluent interfaces even when underlying methods don't support chaining",
      "code_example": "return tap($user, fn($u) => $u->save());",
      "source_file": "docs/laravel/helpers.md",
      "source_lines": "145-152",
      "when_to_apply": ["Building fluent APIs", "Chaining operations with side effects"],
      "when_not_to_apply": ["Method already returns $this"],
      "confidence": 85
    },
    {
      "tier": 4,
      "pattern_name": "PSR-4 vs Classmap Performance",
      "what_i_learned": "PSR-4 autoloading has filesystem overhead; classmap is 10x faster in production",
      "why_it_matters": "Contradicts assumption that autoloading is free — requires explicit optimization",
      "code_example": "composer dump-autoload -o",
      "source_file": "docs/performance/autoloading.md",
      "source_lines": "23-45",
      "anti_pattern": "Using PSR-4 in production without classmap optimization",
      "blast_radius": "Application-wide (every class load)",
      "severity": "high",
      "confidence": 90
    }
  ],
  "focus_filter_applied": false,
  "focus_topic": null,
  "excluded_count": 3
}
```

**Batch rules:**
1. Return JSON only — no prose, no explanations outside JSON
2. 5-10 insights maximum per batch (quality over quantity)
3. Copy code examples verbatim from source (mark if adapted)
4. Include source attribution for every insight
5. Apply focus filter if specified — exclude unrelated insights without comment

## Focus Filter

When `--focus=<topic>` is specified:

- INCLUDE: insights directly related to the topic
- EXCLUDE: unrelated insights (do not summarize to fill space)

Example: `--focus="concurrency"`:
- Include: PHP 8.1 Fibers, ReactPHP event loop, Swoole async
- Exclude: Eloquent ORM, Blade templates, routing patterns

If uncertain whether an insight relates to focus: EXCLUDE.

## Context Efficiency

| Approach | Tokens for 500 files |
|----------|---------------------|
| Direct reading | ~2,500k (exceeds budget) |
| Batch subagents (12 files, JSON response) | ~50k (10x savings) |

Batch size of 12 files is optimal: deep analysis without exhausting subagent context.

## Edge Cases

| Scenario | Response |
|----------|----------|
| >500 files discovered | Warn, cap at 500, suggest `--focus` for remainder |
| Batch returns Tier 1 only | Return `{"insights": [], "excluded_count": N}`, continue |
| Context >90% in coordinator | Stop batching, synthesize what's collected, note files skipped |
| Same pattern in multiple batches | Keep highest confidence, merge when_to_apply |

## Document Template

For the final learning document format, see:
`Read(references/document-template.md)`

## Examples

**Tier classification decision:**

```
Insight: "Use interfaces for testability"
Test: "Could I have written this without reading the docs?" → YES
Decision: Tier 1 — EXCLUDE

Insight: "Laravel's tap() returns the first argument after invoking callback"
Test: "Does this show HOW to do something specific?" → YES
Decision: Tier 2 — INCLUDE

Insight: "Symfony chose synchronous events to enforce explicit async via Messenger"
Test: "Does this explain WHY architects made a trade-off?" → YES
Decision: Tier 3 — INCLUDE

Insight: "DB::transaction() does NOT retry on deadlock — manual retry loop required"
Test: "Does this contradict common assumption?" → YES
Decision: Tier 4 — INCLUDE (highest priority)
```

## Anti-Patterns

| Anti-Pattern | Problem | Fix |
|--------------|---------|-----|
| Including Tier 1 insights | Dilutes signal with training data | Apply "could I have written this?" test |
| Prose in batch output | Bloats subagent response, harder to parse | Enforce JSON-only output |
| More than 10 insights per batch | Quality drops, context fills | Cap at 10, prefer Tier 3-4 |
| Missing source attribution | Cannot verify or locate insight | Always include source_file + source_lines |
| Sequential batch processing | Slow for large doc sets | Dispatch all batches in parallel |

## Success Criteria

- [ ] Every insight passes Tier 2-4 classification test
- [ ] Batch responses are valid JSON with required fields
- [ ] Source attribution present on all insights
- [ ] Focus filter applied if specified
- [ ] Tier 4 insights sorted first in output

## Related Skills

- Skill: tasks-spec-methodology — structure extracted insights into actionable specs
- Skill: next-audit-methodology — codebase analysis that complements documentation learning
