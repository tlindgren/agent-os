# WARP.md

This file provides project-specific rules and guidelines for Warp AI agents when working in this repository.

## Repository Context

This is a personal collection of AI rules, guidelines, and knowledge bases for AI coding tools and custom assistants. The repository serves as a centralized knowledge base for technology-specific rules and coding preferences.

## Architecture

The repository is organized into main areas:

- `rules/planning/` - PRD templates and feature planning guidelines
- `rules/technical/` - Technology-specific rules organized by framework/tool (cloudflare, firebase, react-router-v7, shadcn, tailwind, etc.)
- `rules/claude-gemini-workflow/` - Collaborative workflow templates for Claude ↔ Gemini handoffs
- `rules/user/` - Personal coding preferences and assistant behavior guidelines

## Key Files and Conventions

- All markdown files use kebab-case naming (e.g., `react-router-routing.md`)
- Technology rules are namespaced under their respective directories
- The `coding-assistant.md` file contains core programming principles and conventions

## Coding Preferences

When working with code in this repository or following these guidelines:

- Use kebab-case for file names
- Prefer functional programming paradigms
- Use TypeScript with strong typing
- Prioritize accessibility and performance
- Implement proper error handling and validation
- Use meaningful variable names
- Prefer named exports

## File Naming Conventions

- `*.tsx` for React components
- `*.ts` for utilities, types, and configurations  
- `*.server.ts` for server-side functionality
- All files use kebab-case

## Rubber Duck Mode

When the flag `--rd` is used, enter rubber duck mode: discuss the problem without writing code until the right path forward is established.

## Additional Context

For comprehensive rules and guidelines, also refer to `CLAUDE.md` in this directory, which contains the full specification for AI assistant behavior in this repository.