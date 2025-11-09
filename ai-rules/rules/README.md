# AI Rules Discovery System

*Last updated: 2024-12-10*

This directory contains technology-specific rules and guidelines designed to help AI coding assistants provide better, more accurate assistance. As an AI assistant, use this README to quickly understand which rules apply to the current project and load the appropriate context.

## 🤖 For AI Assistants: Quick Start

### Automatic Technology Detection

1. **Scan the project's `package.json`** to identify installed dependencies
2. **Check this technology matrix** to determine which rules apply
3. **Load relevant rule files** into your working context
4. **Reference the verification checklists** in each rule file

### Manual Technology Specification

When a user says something like:
> "I'm working on a Nextjs app with Tailwind, Convex DB, and third-party auth"

Load the corresponding rule files and adjust your responses accordingly.

## Technology Coverage Matrix

| Technology | Status | Rule Files | Package.json Indicators | Priority |
|------------|--------|------------|------------------------|---------|
| **Next.js** | ✅ Complete | [1 file](./technical/nextjs/) | `next` | High |
| **Tailwind CSS** | ✅ Complete | [1 file](./technical/tailwind/) | `tailwindcss` | High |
| **Convex** | ✅ Complete | [1 file](./technical/convex/) | `convex` | High |
| **Headless UI** | ✅ Complete | [1 file](./technical/headless-ui/) | `@headlessui/react` | High |

## Quick Access by Use Case

### Frontend Development
- **Next.js + Tailwind**: Most common stack
  - Load: [`nextjs-app-router-guidelines.md`](./technical/nextjs/nextjs-app-router-guidelines.md)
  - Load: [`tailwind-nextjs-guidelines.md`](./technical/tailwind/tailwind-nextjs-guidelines.md)

### Full-Stack Development  
- **Next.js + Convex + Tailwind**: Complete modern stack
  - Load: All Next.js rules + Tailwind + [`convex-guidelines.md`](./technical/convex/convex-guidelines.md)

### UI Development
- **Headless UI Components**: When building component libraries
  - Load: [`headless-ui-guidelines.md`](./technical/headless-ui/headless-ui-guidelines.md)
  - Also load: Tailwind rules (Headless UI depends on Tailwind)


## Essential Rules for All Projects

Always load these core guidelines regardless of technology stack:

1. **[Security Best Practices](./user/security-best-practices.md)** - Critical security guidelines
2. **[Coding Assistant Guidelines](./user/coding-assistant.md)** - Core programming principles
3. **[Feature Development Process](./planning/prd-instructions.md)** - Development workflow
4. **[Documentation Structure](./technical/documentation-structure.md)** - Where and how to create project documentation

## Rule File Structure

Each technology rule file follows this structure:
```markdown
---
description: "Brief technology description"
globs: ["file patterns where rules apply"]
last_updated: "YYYY-MM-DD"
reference_url: "link to official docs"
---

# Technology Guidelines

## 🚨 CRITICAL INSTRUCTIONS FOR AI LANGUAGE MODELS 🚨
[Key points AI must absolutely follow]

## Getting Current Documentation
[How to access latest docs - llms.txt URLs when available]

## Key Patterns and Best Practices
[Core implementation patterns with examples]

## Forbidden Patterns  
[What NOT to do with examples]

## AI Model Verification Checklist
[Checklist for AI to verify correct implementation]
```

## Package.json Detection Patterns

Use these patterns to auto-detect technologies:

```javascript
// Example detection logic for AI assistants
const detectTechnologies = (packageJson) => {
  const deps = { ...packageJson.dependencies, ...packageJson.devDependencies };
  const detected = [];
  
  if (deps['next']) {
    detected.push('nextjs');
  }
  
  if (deps['tailwindcss']) {
    detected.push('tailwind');
  }
  
  if (deps['convex']) {
    detected.push('convex');
  }
  
  if (deps['@headlessui/react']) {
    detected.push('headless-ui');
  }
  
  // Add more detection patterns...
  return detected;
};
```

## Integration Examples

### Example 1: New Next.js + Tailwind Project
```bash
# User command
"Set up your memory for a new Next.js project with Tailwind"

# AI should load:
- rules/technical/nextjs/nextjs-app-router-guidelines.md
- rules/technical/tailwind/tailwind-nextjs-guidelines.md
- rules/user/security-best-practices.md
- rules/user/coding-assistant.md
```

### Example 2: Full-Stack Convex Project
```bash
# Package.json contains: convex, next, tailwindcss
# AI should auto-load:
- All Next.js rules
- Tailwind rules  
- Convex guidelines
- Core user rules
```

## Contributing New Rules

When adding new technology rules:

1. **Follow the standard template** structure above
2. **Add detection patterns** to this README
3. **Update the technology matrix** 
4. **Include practical examples** and forbidden patterns
5. **Add verification checklist** for AI assistants
6. **Reference official documentation** with llms.txt URLs when available

## Troubleshooting

### Rules Not Loading
- Check package.json for correct dependency names
- Verify technology matrix mappings are accurate
- Ensure rule files exist and are properly formatted

### Conflicting Guidelines
- Technology-specific rules override general guidelines
- Security rules always take precedence
- When in doubt, prefer explicit over implicit patterns

## Feedback and Updates

This discovery system is designed to evolve. When working on projects:

1. **Note missing rules** for technologies you encounter
2. **Update existing rules** when patterns change
3. **Add new detection patterns** for emerging tools
4. **Improve verification checklists** based on common issues

The goal is to make AI assistants more effective by providing them with the right context automatically, reducing setup time and improving code quality from the start.
