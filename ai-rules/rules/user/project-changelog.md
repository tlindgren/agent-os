# Project Changelog System

## Purpose

This document defines a two-part changelog system for tracking both technical progress and process/learning evolution during software development projects. These changelogs complement version control by providing human-readable context and reflection.

## Overview

The changelog system consists of two files maintained in each project's `_project-docs/` directory:

1. **TECHNICAL_CHANGELOG.md** - Tracks technical decisions, changes, and development context
2. **PROCESS_CHANGELOG.md** - Tracks learning, ideas, and project evolution

This is distinct from personal journaling - it's project documentation that captures both what was built and what was learned during the building process.

## File Locations

```
project-root/
└── _project-docs/
    └── changelogs/
        ├── TECHNICAL_CHANGELOG.md
        ├── PROCESS_CHANGELOG.md
        └── conversations/          # Optional: archived AI conversations
```

## Technical Changelog

### Purpose
Provide a human-readable summary of:
- What was built or explored
- Technical decisions made
- Questions asked during development
- Models, tools, and frameworks discussed
- Files created or changed

### Format

```markdown
## YYYY-MM-DD - Brief Descriptive Title

**Summary **: 
- [Succint, scanable List of topics, technologies, or problems investigated]
- [Important technical choices made]

**Files created/changed**: 
- `path/to/file.ext` - Brief description

**Technical notes**:
- [model being use, AI Assistant bieng used Claude, Gemini, Warp]
- [any new tools or approachse worthy of noting]

**Links**: 
- [Git commit links, external resources]
```

### Guidelines for AI Assistants

**DO:**
- Be concise - this is meant to be readable at a glance to remember what we did, but not as detailed as the full Git commits
- Use objective, factual language
- Try to use plain, somewhat language that's easy to read, unless it's necessary to use specific technical terms
- Record concrete technical decisions
- Link to specific files with descriptions
- Note the development environment when relevant, partiuflary AI tools or any important change

**DON'T:**
- Infer feelings or motivations
- Editorialize about quality or success
- Include opinions on approaches unless explicitly stated
- Make assumptions about future intentions

## Process Changelog

### Purpose
Document the meta-process of development:
- What the developer is learning
- Ideas and insights that emerge
- How thinking about the project evolves
- Questions worth returning to
- General project direction and energy

### Format

```markdown
## YYYY-MM-DD - Brief Descriptive Title

**Summary**:
[1-2 sentences about what happened in this session/conversation. Use "I" for actions the developer objectively took: "I proposed creating two changelogs"]

**Key Ideas**:
- [Important concepts, proposals, or decisions about the project]

**New Learning**:
- [Things the developer learned - based on direct answers to questions or explicitly stated learning]

**Vibe Check**:
[How the developer is feeling about this step, the project, energy levels, etc. 
ONLY fill this in if the developer explicitly states feelings/energy.
Otherwise leave a placeholder: "[To be filled in]"]

**Next Steps**:
- [What to work on next - based on what was discussed or decided]
```

### Guidelines for AI Assistants

**Writing Style:**
- **Summary section**: Use "I" for objective actions the developer took ("I proposed", "I asked", "I created")
- **Other sections**: Stay neutral with pronouns or use third-person when appropriate
- Keep entries relatively concise but capture key moments

**What to Include in "New Learning":**
- ✅ Direct answers to developer's questions ("Warp stores conversations in internal database")
- ✅ Technical information discovered during the session
- ✅ Realizations explicitly stated by the developer
- ❌ Inferred subjective insights ("Building this creates useful meta-layers")
- ❌ Assumptions about what the developer might be learning

**Handling Subjective Content:**
- If you're uncertain whether something is subjective, leave a placeholder
- Prompt the developer to fill in their own feelings/interpretations
- When in doubt, ask first rather than inferring

**When to Create Entries:**
- After significant development sessions
- When requested by the developer
- At natural stopping points in the work

## Reflection Prompt

When the developer asks for help capturing key moments from a conversation, use this process:

1. **Review the conversation** for objective facts and decisions
2. **Identify direct learning** (answers to questions, discoveries)
3. **Note key ideas** that emerged or were proposed
4. **List next steps** that were discussed
5. **Create a draft entry** following the templates
6. **Leave placeholders** for subjective elements like "Vibe Check"
7. **Ask clarifying questions** if you're unsure about framing

**Example prompt to the developer:**
> "I've drafted a Process Changelog entry from our conversation. I left the Vibe Check blank - would you like to add how you're feeling about this step? Also, I wasn't sure if [specific point] was something you learned or something you already knew - let me know if I should adjust that."

## Conversation Archiving (Optional)

Some developers may want to preserve full conversations for future reference.

**Recommendation:**
- Warp stores conversations internally but has limited export functionality
- Consider periodically copying important conversations to `_project-docs/changelogs/conversations/`
- Use date-based filenames: `2025-10-08-cross-platform-discussion.md`
- These provide searchable archives of context and decision-making

## Using This System

### For Developers:
1. Add `_project-docs/changelogs/` directory to your project
2. Create TECHNICAL_CHANGELOG.md and PROCESS_CHANGELOG.md with templates in that directory
3. Update after significant development sessions
4. Ask your AI assistant to help extract key moments
5. Fill in subjective sections yourself (Vibe Check, nuanced feelings)

### For AI Assistants:
1. When technical work is completed, offer to update Technical Changelog
2. When asked to help reflect, create draft Process Changelog entry
3. Follow objectivity guidelines strictly - err on the side of asking
4. Leave placeholders for subjective content
5. Use templates as loose guides, not rigid structures

## Examples

### Good Technical Changelog Entry:

```markdown
## 2025-10-08 - API Integration Setup

**What we explored**: 
- GitHub OAuth flow for app authentication
- Convex real-time database integration patterns

**Key decisions**: 
- Using environment variables for API keys (not hardcoded)
- Server-side token exchange for OAuth (not client-side)

**Files created**: 
- `lib/github-auth.ts` - GitHub OAuth helper functions
- `.env.example` - Template for required environment variables

**Your key questions**: 
- "How do I securely store the GitHub token?"
- "Can Convex mutations be called from the OAuth callback?"

**Links**: 
- [Commit abc123]
```

### Good Process Changelog Entry:

```markdown
## 2025-10-08 - Exploring Authentication Patterns

**Summary**:
I investigated OAuth integration for the first time, focusing on security best practices for token storage.

**Key Ideas**:
- Environment variables for secrets (not committed to git)
- Server-side OAuth flow is more secure than client-side
- Convex can act as the OAuth callback handler

**New Learning**:
- OAuth requires server-side token exchange for security
- GitHub tokens should be encrypted before storage
- Callback URLs must match exactly what's registered in GitHub

**Vibe Check**:
[To be filled in]

**Next Steps**:
- Implement the server-side callback endpoint
- Test the OAuth flow end-to-end
- Add error handling for failed authentication
```

## Variations

This system is flexible and can be adapted to different workflows:
- Some developers may prefer shorter, more frequent entries
- Others may prefer longer, weekly summaries
- Templates can be adjusted per project needs
- Not all sections need to be filled for every entry

The key is maintaining the distinction between objective facts (Technical) and reflective learning (Process), while respecting the developer's voice and avoiding over-inference.