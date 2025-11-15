# Documentation Structure Guidelines

This document provides clear guidelines for where and how to create documentation in Design Tools projects.

## Documentation Hierarchy

### Project-Level Documentation

#### `_project-docs/technical/`
**Purpose**: Cross-component architecture, system design, and integration documentation

**When to Use**:
- System architecture documentation
- Cross-component integration guides
- Technical specifications that affect multiple components
- Testing strategies and development workflows
- API documentation and data models

**Naming Convention**: ALL-CAPS with hyphens
- ✅ `CANVAS-PREVIEW-SYSTEM.md`
- ✅ `TESTING-STRATEGY.md`
- ✅ `DATABASE-SCHEMA.md`
- ❌ `testing-guidelines.md`
- ❌ `canvas_preview.md`

**Required Format**:
```markdown
# System Name Documentation

**Created**: [Date]
**Status**: ✅ Complete | 🚧 In Progress | 🛑 Blocked
**Branch**: [branch name]
**Components**: [key technologies/components]

## Overview
[Brief description of the system/strategy]

## Architecture
[Detailed technical information]
```

#### `_project-docs/planning/`
**Purpose**: Feature planning, PRDs, and AI assistant handoff logs

**Structure**: `_project-docs/planning/[feature-name]/`
- `prd.md` - Product requirements document
- `handoff-log.md` - AI assistant collaboration log

**When to Use**: Following Claude ↔ Gemini workflow for new features

### Component-Level Documentation

#### Component README Only
**Location**: `[component-directory]/README.md`

**When to Use**:
- Component-specific setup instructions
- Local development notes
- Component-specific configuration

**What NOT to Do**:
- ❌ Do NOT create `[component]/docs/` directories
- ❌ Do NOT duplicate project-level documentation
- ❌ Do NOT create component-specific technical architecture docs

## Decision Matrix

| Documentation Type | Location | Example |
|-------------------|----------|---------|
| Testing Strategy | `_project-docs/technical/` | `TESTING-STRATEGY.md` |
| Database Design | `_project-docs/technical/` | `DATABASE-SCHEMA.md` |
| API Integration | `_project-docs/technical/` | `CANVAS-API-INTEGRATION.md` |
| Component Setup | `theme-manager/README.md` | Setup and run instructions |
| Feature Planning | `_project-docs/planning/auth/` | `prd.md`, `handoff-log.md` |

## AI Assistant Guidelines

### For Claude
When creating technical documentation:
1. **Always use** `_project-docs/technical/` for system-level docs
2. **Follow ALL-CAPS naming** with metadata header
3. **Check existing docs** for naming patterns before creating new files

### For Gemini  
When implementing features:
1. **Reference existing** technical documentation in `_project-docs/technical/`
2. **Update handoff logs** in planning directories
3. **Do NOT create** new technical documentation - refer to Claude for doc creation

## Common Mistakes to Avoid

❌ **Creating component docs directories**:
```
theme-manager/docs/testing.md  ← Wrong
```

✅ **Using project technical docs**:
```  
_project-docs/technical/TESTING-STRATEGY.md  ← Correct
```

❌ **Mixing naming conventions**:
```
_project-docs/technical/testing-guidelines.md  ← Wrong
```

✅ **Consistent ALL-CAPS naming**:
```
_project-docs/technical/TESTING-STRATEGY.md  ← Correct
```

❌ **Missing metadata headers**:
```markdown
# Testing Guidelines
This document explains testing...
```

✅ **Proper format with metadata**:
```markdown  
# Testing Strategy Documentation

**Created**: October 7, 2025
**Status**: ✅ Complete
**Branch**: main
**Components**: Jest, React Testing Library

## Overview
This document outlines...
```

## Rationale

This structure ensures:
- **Centralized knowledge**: All technical docs in one discoverable location
- **Consistent naming**: Easy to find and reference documentation
- **Clear ownership**: Project-level vs component-level documentation boundaries
- **AI collaboration**: Clear guidelines for Claude (planning) vs Gemini (implementation)