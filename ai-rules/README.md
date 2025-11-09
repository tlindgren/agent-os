# AI Rules Repository

This is a personal collection of AI rules, guidelines, and knowledge bases specifically tailored for AI coding tools and custom assistants. It's designed to enhance a workflow when using AI systems with different technology stacks, but it's public in case others find it useful.

This project was originally forked from [explorer-ai-rules](https://github.com/fidalgok/explorer-ai-rules) by Kyle Fidalgo.

## Purpose

## Quick Start

**Get AI rules working in your project in 3 steps:**

1. **Add as a submodule:**
   ```bash
   git submodule add https://github.com/tlindgren/ai-rules.git .ai-rules
   git commit -m "Add AI rules submodule"
   ```

2. **Initialize on clone** (for team members):
   ```bash
   git clone --recurse-submodules <your-project-repo>
   # Or if already cloned:
   git submodule update --init
   ```

3. **Reference in `CLAUDE.md`** (see templates below)

**That's it!** AI assistants will now have access to comprehensive coding guidelines.

The main purpose of this repository is to:

- Store and organize AI-specific rules and guidelines for my projects
- Maintain knowledge bases for different technology stacks I work with
- Provide consistent interaction patterns with AI coding assistants
- Enable better collaboration between myself and AI tools
- Document best practices for AI-assisted development that work for me

## Repository Structure

```bash
.
├── rules/                   # AI rules and guidelines
│   ├── planning/          # Feature development guidelines
│   ├── technical/         # Technology-specific rules
│   │   ├── convex/          # Convex backend platform
│   │   ├── headless-ui/     # Headless UI guidelines
│   │   ├── nextjs/          # Next.js (React) guidelines
│   │   └── tailwind/        # Tailwind CSS framework
│   └── user/                # User-specific rules and preferences
│       ├── coding-assistant.md      # Core programming principles
│       └── security-best-practices.md # Security guidelines for AI assistants
└── CLAUDE.md                # Instructions for Claude Code
```

## Usage

### Option 1: Git Submodule Integration (Recommended)

To integrate these AI rules into your existing projects:

1. **Add as Submodule**

   ```bash
   # In your project root directory
   git submodule add https://github.com/tlindgren/ai-rules.git .ai-rules
   git commit -m "Add AI rules submodule"
   ```

2. **Reference in Your Project's CLAUDE.md**

   ```markdown
   # AI Rules Integration

   This project includes comprehensive AI coding guidelines from [.ai-rules/](./.ai-rules/)

   For technology-specific rules, see:

   - [Security Best Practices](./.ai-rules/rules/user/security-best-practices.md)
   ```

3. **Update Rules in Any Project**

   ```bash
   # Pull latest AI rules updates
   git submodule update --remote .ai-rules
   git add .ai-rules
   git commit -m "Update AI rules to latest version"
   ```

4. **Clone Projects with Submodules**

   ```bash
   # When cloning projects that use this submodule
   git clone --recurse-submodules <your-project-repo>

   # Or if already cloned
   git submodule init
   git submodule update
   ```

5. **Contributing Changes Back from Submodule**

   When working in a project and you need to update AI rules based on your work:

   ```bash
   # Navigate to the submodule directory
   cd .ai-rules

   # Make your changes to the AI rules files
   # Edit rules/technical/your-framework/your-file.md

   # Commit changes in the submodule
   git add .
   git commit -m "Update AI rules based on project work"

   # Push to the AI rules repository
   git push origin main

   # Go back to parent project and update submodule reference
   cd ..
   git add .ai-rules
   git commit -m "Update AI rules submodule reference"
   ```

   This workflow allows you to evolve AI rules based on real project experience while keeping them centralized for use across all your projects.

### Managing This Repository

1. **Adding New Rules**

   - Create new rule files in the appropriate directory under `/rules/technical/`
   - Follow kebab-case naming conventions
   - Include clear documentation and examples

2. **Technology Stack Organization**

   - Each technology has its own directory under `/rules/technical/`
   - Include llms.txt references for up-to-date documentation
   - Keep rules specific to each technology stack

3. **User Rules**
   - Store personal preferences under `/rules/user/`
   - Include coding style preferences, security guidelines, etc.

## Customization

While `.ai-rules` provides a global set of rules, your project will likely have its own specific conventions.

### When to Use Project-Specific Documentation

**Add to your project's `CLAUDE.md` when:**
- Convention is specific to your codebase/architecture
- Pattern is too niche to benefit other projects
- Rule overrides a `.ai-rules` guideline for project-specific reasons

**Consider contributing to `.ai-rules` when:**
- Pattern applies to a widely-used framework/technology
- Convention solves a common problem across projects
- Guidance would help other teams using the same stack

### How to Document Project-Specific Rules

1. **Create `CLAUDE.md`** in your project root (use our template)
2. **Add "AI Rules Integration" section** referencing this submodule
3. **Add "Project-Specific Overrides" section** with your custom conventions
4. **Link to detailed docs** in your project's `_project-docs/` or similar

### Example Override Documentation

```markdown
### Project-Specific Overrides

**Import/Export Patterns:** While `.ai-rules` prefers named exports, this project uses **default exports for Next.js page components**. This is the framework's standard convention and must be followed.

**File Structure:** This project uses a monorepo structure with `/packages` directory. See `docs/architecture.md` for details.
```

## Contributing

While this is primarily a personal repository that I maintain for my own use, I appreciate suggestions and ideas. If you find this useful and have improvements to suggest:

- Feel free to fork the repository for your own use
- Open an issue if you find errors or have suggestions
- Submit pull requests if you'd like to contribute improvements

## License

MIT License

Copyright (c) 2024 Tim Lindgren

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Contact

This is a personal project maintained by [tlindgren](https://github.com/tlindgren). If you have questions or suggestions, please open an issue in this repository.
