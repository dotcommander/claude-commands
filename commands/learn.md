---
description: ingest knowledge from external sources using batch processing
argument-hint: "<url|file|directory> [compare <url|file>] [--output=<path>] [--focus=<topic>]"
allowed-tools: Task, Skill
---

## Dispatch

```
compare → Task(learn-agent): compare $ARGUMENTS
*       → Task(learn-agent): extract insights from $ARGUMENTS
```

## Example

```bash
/learn docs/languages/go
/learn https://go.dev/doc/go1.26
/learn docs/frameworks/svelte --output=~/learnings/svelte.md
/learn vendor/package/src --focus="concurrency patterns"
/learn compare https://other-project.dev/docs
```

## Success Criteria

- [ ] learn-agent completed successfully
- [ ] Insights extracted via parallel batch processing (Tier 2-4 only)
- [ ] Tier 1 (training data) filtered out
- [ ] Final learning document written to output path
