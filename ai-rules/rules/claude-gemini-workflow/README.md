# Claude ↔ Gemini Workflow Guidelines

This directory contains **templates and documentation** for Claude-Gemini collaboration workflow.

**This is a Git submodule - READ-ONLY templates only. Never write to this directory.**

---

## Directory Structure

**In this submodule (.ai-rules/):**
```
.ai-rules/rules/claude-gemini-workflow/
├── README.md                   # This file
├── QUICK-START.md              # Tutorial
└── templates/
    ├── prd-template.md         # Feature spec template
    └── handoff-log-template.md # Build/review log template
```

**In your project (_project-docs/):**
```
_project-docs/
├── planning/
│   └── feature-name/
│       ├── prd.md              # Claude writes spec
│       └── handoff-log.md      # Both update (chronological)
└── technical/
    ├── architecture.md
    └── database-schema.md
```

---

## Quick Reference

### Starting a New Feature

1. **Claude Plans:**
   ```
   User: "Plan the [feature] using template at .ai-rules/rules/claude-gemini-workflow/templates/prd-template.md"
   Claude: Creates _project-docs/planning/[feature-name]/prd.md
   Claude: Creates _project-docs/planning/[feature-name]/handoff-log.md (initial entry)
   ```

2. **Gemini Builds:**
   ```
   User: "Read _project-docs/planning/[feature-name]/prd.md and implement it"
   Gemini: Builds code, updates _project-docs/planning/[feature-name]/handoff-log.md
   ```

3. **Claude Reviews:**
   ```
   User: "Review [feature] against _project-docs/planning/[feature-name]/prd.md"
   Claude: Reviews code, updates _project-docs/planning/[feature-name]/handoff-log.md
   ```

4. **Iterate or Ship**

---

## Templates in This Directory

### `templates/prd-template.md`
- Template for writing feature specifications (PRDs)
- Claude copies this to `_project-docs/planning/[feature-name]/prd.md`
- Contains: purpose, scope, acceptance criteria, technical design

### `templates/handoff-log-template.md`
- Template for tracking Claude ↔ Gemini collaboration
- Both AIs update chronologically in `_project-docs/planning/[feature-name]/handoff-log.md`
- Shows: planning → building → review → fixes → ship

---

## Workflow Summary

```
Claude (plans)
  → creates _project-docs/planning/[feature]/prd.md
  → creates _project-docs/planning/[feature]/handoff-log.md (initial entry)

Gemini (builds)
  → reads prd.md
  → writes code
  → updates handoff-log.md (build complete entry)

Claude (reviews)
  → reads prd.md + code
  → checks quality
  → updates handoff-log.md (review entry)

Iterate or Ship
```

---

## When to Use This

**Use this workflow when:**
- Feature is complex (multi-file, many edge cases)
- You want to leverage both AIs' strengths
- Code quality is critical
- You want documentation of decisions

**Skip this workflow when:**
- Quick bug fix (just use one AI)
- Experimental prototype
- One-line change

---

## Tips

**For Best Results:**
1. Make specs detailed (the more specific, the better Gemini builds)
2. Update logs immediately (don't batch)
3. Be honest in reviews (constructive criticism improves quality)
4. Iterate when needed (don't force shipping broken code)

---

## Related Documentation

- **QUICK-START.md**: 5-minute tutorial for first feature
- **Templates**: Copy from `templates/` directory
- **Project Structure**: See top of this file

---

**Important:** This is a Git submodule. Never write files here. All project-specific work goes in `_project-docs/` in your project root.
