# Custom Profile

This profile contains your migrated coding standards and technical guidelines, consolidated from the ai-rules submodule into Agent OS's profile system.

## What This Profile Contains

### Standards Organization

```
standards/
├── global/
│   ├── tech-stack.md              # Your default tech stack
│   ├── coding-style.md            # Coding conventions and best practices
│   └── security-best-practices.md # Security guidelines for AI assistants
├── frontend/
│   ├── nextjs.md                  # Next.js App Router guidelines
│   ├── tailwind.md                # Tailwind CSS guidelines
│   └── headless-ui.md             # Headless UI component patterns
└── backend/
    └── convex.md                   # Convex backend guidelines
```

### Key Changes from ai-rules

**Before (ai-rules submodule):**
- Maintained as separate Git submodule
- Required adding to each project
- Technology-specific rules in nested directories
- Referenced from project CLAUDE.md/WARP.md files

**After (Agent OS custom profile):**
- Integrated into Agent OS profile system
- Automatically included when using this profile
- Standards organized by category (global, frontend, backend, testing)
- Available to all Agent OS workflows and agents

## Usage

### On New Projects

```bash
cd /Users/tim/agent-os
./scripts/project-install.sh --profile custom /path/to/your/project
```

This installs Agent OS with your custom standards automatically included.

### Standards Inheritance

This profile inherits from the `default` profile, which means:
- You get all default Agent OS workflows and agents
- Your custom standards override/extend the defaults
- You only need to define what's different from default

Files in `profiles/custom/` take precedence over `profiles/default/`.

## Customizing for Specific Projects

### Project-Level Overrides

After installing Agent OS in a project, you can customize standards for that specific project:

```
your-project/agent-os/standards/
├── global/
│   └── tech-stack.md    # Override for this project only
└── ...
```

Project-level standards override profile standards.

### When to Create Project Standards

**Use profile standards (here) for:**
- Conventions that apply to all your projects
- General best practices
- Default tech stack

**Use project standards for:**
- Project-specific architectural decisions
- One-off technology choices
- Client-specific requirements

## Migrated Content

### From ai-rules/rules/user/
- ✅ `coding-assistant.md` → `standards/global/coding-style.md`
- ✅ `security-best-practices.md` → `standards/global/security-best-practices.md`

### From ai-rules/rules/technical/
- ✅ `nextjs/nextjs-app-router-guidelines.md` → `standards/frontend/nextjs.md`
- ✅ `tailwind/tailwind-nextjs-guidelines.md` → `standards/frontend/tailwind.md`
- ✅ `headless-ui/headless-ui-guidelines.md` → `standards/frontend/headless-ui.md`
- ✅ `convex/convex-guidelines.md` → `standards/backend/convex.md`

## Tech Stack

Default stack for this profile (customize in `standards/global/tech-stack.md`):

- **Frontend:** Next.js 15 + React 19 + Tailwind CSS + Headless UI
- **Backend:** Convex
- **Language:** TypeScript
- **Package Manager:** pnpm

## Benefits of This Approach

### vs. ai-rules Submodule

**Advantages:**
- ✅ Single system (no submodule management)
- ✅ Automatic inclusion in all Agent OS workflows
- ✅ Integrated with spec-writer, product-planner agents
- ✅ Standards compilation into project-specific docs
- ✅ No need to manually reference rules files

**Trade-offs:**
- Changes are profile-specific (not shared via submodule)
- Need to update profile directly instead of central repo
- Good for personal use, less ideal for team sharing

### For Team Sharing

If you need to share standards across a team:
1. Fork this agent-os repo
2. Team members clone your fork
3. Everyone uses the `custom` profile
4. Standards updates via Git pull

## Workflows Integration

All Agent OS workflows and agents now automatically have access to your standards:

### Spec Writer Agent
When creating specifications, automatically references:
- Tech stack from `standards/global/tech-stack.md`
- Coding conventions from `standards/global/coding-style.md`
- Framework guidelines from `standards/frontend/nextjs.md`

### Implementation Workflows
When implementing features, agents follow:
- Security best practices
- Coding style guidelines
- Framework-specific patterns

### Verification Workflows
When verifying implementations, agents check:
- Code adheres to standards
- Security practices followed
- Proper patterns used

## Updating Standards

### To Update Profile Standards

```bash
cd /Users/tim/agent-os
# Edit files in profiles/custom/standards/
# Commit changes
git add profiles/custom
git commit -m "Update coding standards"
```

### To Update Standards in Existing Projects

After updating profile standards, re-run installation:

```bash
cd /Users/tim/agent-os
./scripts/project-install.sh --profile custom /path/to/existing/project
```

This updates the compiled standards in that project's `agent-os/standards/` folder.

## Migration Checklist

- [x] Migrated general coding guidelines
- [x] Migrated security best practices
- [x] Migrated Next.js guidelines
- [x] Migrated Tailwind CSS guidelines
- [x] Migrated Headless UI guidelines
- [x] Migrated Convex guidelines
- [x] Created tech stack definition
- [x] Updated tech stack to use Headless UI
- [ ] Test on new project
- [ ] Update existing projects

## Next Steps

1. **Test the profile** on a new project
2. **Migrate additional** framework guidelines as needed
3. **Customize tech-stack.md** if your defaults differ
4. **Update existing projects** to use this profile

## Questions?

See:
- **Agent OS Docs:** https://buildermethods.com/agent-os
- **Default Profile:** `/Users/tim/agent-os/profiles/default/` for reference
- **Project Structure:** Generated in each project's `agent-os/` folder
