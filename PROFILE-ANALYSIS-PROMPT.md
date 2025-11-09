# Agent OS Profile Analysis Prompt

Use this prompt when analyzing a project to determine if it needs a custom Agent OS profile.

---

## Context

I'm using Agent OS (https://buildermethods.com/agent-os) for structured software development workflows. Agent OS uses profiles to define coding standards, tech stacks, and development patterns.

**Current profiles:**
- `default` - General web development profile from Agent OS
- `custom` - My Next.js/React/TypeScript/Convex/Headless UI profile (located at `/Users/tim/agent-os/profiles/custom/`)

## Task

I have a project that may need its own Agent OS profile. Please analyze the project and help me determine:

1. **Does this project need a separate profile?** Or can it use an existing one?
2. **What standards should be included?** Based on the project's tech stack and conventions
3. **Should I create a new profile or modify the custom profile?**

## Project Information

**Project directory:** [I'll provide this]

**Key files to analyze:**
- `.claude/project.md` - Project overview and context
- `.claude/technical-context.md` - Technical stack and architecture
- `.claude/ai-guidelines.md` - Existing AI coding guidelines
- `README.md` - Project documentation
- `pyproject.toml` / `package.json` - Dependencies
- Source code structure

## What I Need

Based on the project analysis, provide:

### 1. Profile Recommendation

**Option A: Use existing custom profile**
- When: Project uses Next.js/React/TypeScript stack similar to custom profile
- Action: Just install Agent OS with `--profile custom`

**Option B: Create new profile (e.g., `python-ml`)**
- When: Significantly different tech stack (e.g., Python/ML vs TypeScript/React)
- Action: Create new profile in `/Users/tim/agent-os/profiles/[name]/`

**Option C: Extend custom profile**
- When: Mostly similar but needs additional standards
- Action: Add standards to custom profile

### 2. Profile Structure

If creating a new profile, suggest:

```
profiles/[profile-name]/
├── README.md
├── profile-config.yml
└── standards/
    ├── global/
    │   ├── tech-stack.md
    │   ├── coding-style.md
    │   └── [other global standards]
    ├── backend/ or python/ or [language-specific]/
    │   └── [framework-specific standards]
    └── [other categories]/
```

### 3. Standards to Migrate

List what should be migrated from existing `.claude/` files:
- Which sections from `project.md`
- Which sections from `technical-context.md`
- Which sections from `ai-guidelines.md`
- What format they should take in Agent OS

### 4. Migration Steps

Provide specific steps:
1. Create profile structure
2. Create specific standards files
3. Populate with content from `.claude/` files
4. Test the profile
5. Update project to use the profile

## Example Analysis Format

```markdown
## Profile Analysis: [Project Name]

### Tech Stack Detected
- Primary language: [e.g., Python 3.11]
- Framework: [e.g., FastAPI, Django]
- Database: [e.g., PostgreSQL]
- Frontend: [if any]
- Testing: [e.g., pytest]

### Recommendation: [Option A/B/C]

**Reasoning:**
[Explain why this option makes sense]

### Profile Name (if new): `[profile-name]`

### Standards to Create

1. **`standards/global/tech-stack.md`**
   - Migrate from: [specific sections]
   - Content: [summary]

2. **`standards/python/[framework].md`**
   - Migrate from: [specific sections]
   - Content: [summary]

[etc.]

### Migration Commands

```bash
# Create profile structure
mkdir -p /Users/tim/agent-os/profiles/[name]/standards/{global,python}

# Create files
[specific commands]

# Test installation
cd /Users/tim/agent-os
./scripts/project-install.sh --profile [name] /path/to/project
```

### Notes
[Any special considerations, conflicts with existing profiles, etc.]
```

## Additional Context

**Agent OS Profile Features:**
- Inherits from `default` profile (has workflows for planning, specs, implementation)
- Standards are automatically included in all AI workflows and agents
- Profiles can be tech-stack specific (e.g., one for Python/ML, one for TypeScript/React)
- Standards organized by category: global, frontend, backend, testing, etc.

**My Current custom Profile Has:**
- Next.js 15 (App Router)
- React 19 (Server Components)
- TypeScript
- Tailwind CSS
- Headless UI (@headlessui/react)
- Convex backend
- Security best practices
- Coding style conventions

---

## Ready to Analyze

When I provide the project directory and you've read the `.claude/` files, please provide the complete analysis in the format above.
