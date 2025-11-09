# Reflective OS Profile

Agent OS profile for **Reflective OS**: A Python-based data science project analyzing 33 years of personal journaling with a Next.js dashboard for visualization.

## Profile Overview

**Best for:** Python data science + NLP analysis with secondary Next.js dashboard

**Tech Stack:**
- **Primary Backend:** Python 3.x (pandas, NLTK, scikit-learn, gensim)
- **Secondary Frontend:** Next.js 15 + React 19 + TypeScript
- **Data Layer:** Pickle, CSV, JSON files
- **Visualization:** Recharts, matplotlib, seaborn
- **Integration:** Obsidian Smart Connections embeddings

## Standards Included

### Global Standards
- **`tech-stack.md`** – Python + Next.js tech stack definition
- **`coding-style.md`** – Python naming conventions, code structure, TypeScript patterns

### Backend Standards
- **`python-data-science.md`** – Data handling, validation, preprocessing, file output formats
- **`analysis-methods.md`** – Computational vs. interpretive distinction, framework application, method limitations

### Frontend Standards
- **`nextjs-dashboard.md`** – Dashboard architecture, data loading, Recharts visualization patterns

### Testing Standards
- **`python-testing.md`** – pytest organization, fixtures, unit/integration tests, edge cases

## Installation

```bash
cd /Users/tim/agent-os
./scripts/project-install.sh --profile reflective-os /path/to/your/project
```

This copies all standards to your project's `.agent-os/standards/` directory.

## Key Patterns This Profile Emphasizes

### 1. Python Data Pipeline
- Numbered scripts (01, 02, 03...) for execution order
- Intermediate `.pkl` files to avoid re-computation
- Data validation at each step
- Preprocessing documentation

### 2. Data Science Best Practices
- Validation before processing
- Explicit method limitations
- Computational vs. interpretive distinction
- Metadata in all outputs (charts, files, reports)

### 3. Analysis Methods
- Clear separation: what computation shows vs. what requires interpretation
- Framework application with humility (Fowler's Stages, attachment theory, etc.)
- Handling unexpected patterns
- Exploratory research vs. hypothesis testing

### 4. Python + TypeScript Integration
- Python produces: `.csv`, `.json`, `.pkl` files
- TypeScript consumes: loads and visualizes
- Consistent naming across both languages

### 5. Testing Strategy
- Unit tests for functions
- Integration tests for pipeline
- Fixtures for test data
- Mocking Obsidian vault data

## Project Structure Convention

```
project/
├── Analysis/
│   ├── scripts/           # 01_data_loader.py, 02_eda.py, etc.
│   ├── data/              # Generated .pkl, .csv, .json files
│   ├── visualizations/    # Generated PNG charts
│   ├── reports/           # Interpretive analysis markdown
│   └── tests/             # pytest test files
├── dashboard/             # Next.js app (optional)
│   ├── app/
│   ├── components/
│   └── public/data/       # Symlink to ../Analysis/data/
├── docs/                  # Conceptual documentation
└── .claude/               # Project context for AI assistants
    ├── project.md
    ├── technical-context.md
    └── ai-guidelines.md
```

## Using This Profile

### For Claude Code Development
When working on Python analysis scripts:
- Follow numbered script pattern (01, 02, 03...)
- Reference `standards/backend/python-data-science.md` for data handling
- Reference `standards/backend/analysis-methods.md` for computational approach
- Include validation, documentation, and test patterns

### For Dashboard Development
When building Next.js visualization:
- Reference `standards/frontend/nextjs-dashboard.md` for data loading
- Follow Recharts component patterns
- Load from `public/data/` symlink
- Include metadata and accessibility in charts

### For Testing
When writing tests:
- Use pytest (see `standards/testing/python-testing.md`)
- Create fixtures for test data
- Mock Obsidian vault when needed
- Test data validation, edge cases, performance

## Profile Features

### Inherits From
This profile inherits from the `default` Agent OS profile, so you get:
- All default workflows and agents
- Default testing and frontend patterns
- Your custom standards override defaults where applicable

### Standards Injection
Standards are automatically available to:
- Claude Code (as Claude Code Skills)
- Spec Writer Agent (when creating specifications)
- Product Planner (when planning features)
- Implementation workflows

## When to Use This Profile

✅ **Use this profile if:**
- Your project is Python-based (data science, analysis, NLP)
- You have secondary frontend (dashboards, visualization)
- You work with Jupyter notebooks, pandas, scikit-learn
- You need clear computational vs. interpretive distinction

❌ **Don't use this profile if:**
- You're building a pure Next.js/React app (use `custom` profile)
- You're doing general web development (use `default` profile)
- Your Python work doesn't involve data science

## Customizing for Your Project

### Project-Level Overrides

After installation, customize standards in your project:

```
your-project/.agent-os/standards/
├── global/
│   └── tech-stack.md    # Override default stack
└── backend/
    └── [project-specific].md
```

Project-level standards override profile standards.

### When to Customize

**Use profile standards for:**
- Default practices across all data science projects
- General Python conventions
- Common testing patterns

**Use project standards for:**
- Project-specific tech choices
- Custom analysis workflows
- Domain-specific patterns

## Features & Examples

### Computational vs. Interpretive
See `standards/backend/analysis-methods.md` for guidance on:
- What computation can reliably show
- What requires human interpretation
- How to frame findings appropriately
- Framework application with limitations

### Data Science Pipeline
See `standards/backend/python-data-science.md` for:
- Data validation patterns
- Preprocessing documentation
- Intermediate file persistence
- Visualization best practices

### Python + TypeScript Bridge
See `standards/frontend/nextjs-dashboard.md` for:
- Loading Python outputs in TypeScript
- Recharts visualization patterns
- File format standards (CSV, JSON, PKL)
- Performance considerations

## Questions & Troubleshooting

### "Can I use this for pure Python ML projects?"
Yes! Just skip the dashboard standards and focus on backend/testing standards.

### "What if I need different standards?"
Create project-level overrides in `.agent-os/standards/`, or fork this profile to create a specialized variant.

### "Does this work with Jupyter notebooks?"
This profile is designed for scripts (01_script.py, etc.). For notebook-based work, create a separate profile or project standards.

## Next Steps

1. **Install profile** – Run project-install.sh
2. **Review standards** – Read through `.agent-os/standards/` files
3. **Update tech-stack.md** – Confirm Python/Next.js versions match your project
4. **Start development** – Claude Code now has standards context

## For More Information

- **Agent OS Docs:** https://buildermethods.com/agent-os
- **Profile Config:** See `profile-config.yml` in this directory
- **Default Profile:** `/Users/tim/agent-os/profiles/default/` for reference

---

**Profile created for:** Reflective OS project (personal journaling analysis with ML/NLP)

**Maintained by:** Tim (tim@buildermethods.com)

**Last updated:** 2025-11-09
