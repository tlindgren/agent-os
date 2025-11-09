# Quick Start: Claude ↔ Gemini Workflow

**30-Second Overview for New Users**

---

## The Pattern

```
1. Claude PLANS   → writes spec to _project-docs/planning/[feature]/
2. Gemini BUILDS  → writes code, updates handoff log
3. Claude REVIEWS → checks quality, updates handoff log
4. Iterate or Ship
```

---

## Your First Feature (5 Minutes)

### Step 1: Claude Plans

**You say:**
> "Plan a user authentication feature. Use the PRD template from `.ai-rules/rules/claude-gemini-workflow/templates/prd-template.md` and create it in `_project-docs/planning/user-auth/`"

**Claude does:**
- Creates `_project-docs/planning/user-auth/` directory
- Reads template
- Writes detailed spec to `_project-docs/planning/user-auth/prd.md`
- Copies handoff log template to `_project-docs/planning/user-auth/handoff-log.md`
- Adds initial entry to handoff log

---

### Step 2: Gemini Builds

**You say:**
> "Read `_project-docs/planning/user-auth/prd.md` and implement it. Update the handoff log when done."

**Gemini does:**
- Reads spec
- Creates component files
- Writes code
- Updates `_project-docs/planning/user-auth/handoff-log.md` with build details

---

### Step 3: Claude Reviews

**You say:**
> "Review the user auth feature against `_project-docs/planning/user-auth/prd.md` and document findings in the handoff log."

**Claude does:**
- Reads spec + code
- Checks quality
- Finds issues
- Updates `_project-docs/planning/user-auth/handoff-log.md` with review

---

### Step 4: Fix or Ship

**If issues found:**
> "Gemini: Read Claude's review in `_project-docs/planning/user-auth/handoff-log.md` and fix the CRITICAL items"

**If looks good:**
> Test it yourself, then ship! 🚀

---

## That's It!

You now have:
- ✅ Detailed spec (documentation)
- ✅ Working code (implementation)
- ✅ Quality review (validation)
- ✅ Complete audit trail (handoff log)

**All organized in one feature folder.**

---

## Directory Structure

```
_project-docs/
└── planning/
    └── user-auth/              # One folder per feature
        ├── prd.md              # Claude writes spec
        └── handoff-log.md      # Both update chronologically
```

---

## Key Files

| File | Who Writes | Purpose |
|------|-----------|---------|
| `_project-docs/planning/[feature]/prd.md` | Claude | What to build |
| `_project-docs/planning/[feature]/handoff-log.md` | Both | Build + review history |

---

## Common Prompts

### To Claude (Planning):
```
"Plan [feature name] using the PRD template. Create in _project-docs/planning/[feature-name]/"
```

### To Gemini (Building):
```
"Read _project-docs/planning/[feature-name]/prd.md and implement it"
```

### To Claude (Reviewing):
```
"Review [feature] against _project-docs/planning/[feature-name]/prd.md"
```

### To Gemini (Fixing):
```
"Read the latest review in _project-docs/planning/[feature-name]/handoff-log.md and fix issues"
```

---

## Next Steps

1. ✅ Try the example above
2. 📖 See `README.md` for more details
3. 📁 Browse `templates/` for PRD and handoff log templates
4. 🚀 Build real features!

---

## Why This Works

**Claude is great at:**
- System design
- Anticipating edge cases
- Code review

**Gemini is great at:**
- Fast implementation
- Following detailed specs
- Data processing

**Together:** Better than either alone.

---

## Important Notes

- `.ai-rules/` is a Git submodule (read-only templates)
- `_project-docs/` is project-specific (your work goes here)
- Each feature gets its own folder for clean organization
- Handoff log keeps full conversation history in one place

---

**Questions?** See `README.md` in this directory.

**Ready to build?** Start with Step 1 above! 🎯
