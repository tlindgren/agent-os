# Tech Stack: Reflective OS

## Primary Backend: Python Data Science & Analysis

### Python Environment
- **Language:** Python 3.x
- **Package Manager:** pip or venv
- **Primary Domain:** NLP, computational text analysis, data science

### Data Processing & NLP Libraries
- **Data Manipulation:** pandas, numpy
- **NLP & Text Processing:** nltk, spacy, gensim
- **Topic Modeling:** scikit-learn (LDA)
- **Sentiment Analysis:** vaderSentiment
- **Network Analysis:** networkx
- **Data Persistence:** pickle (.pkl files for intermediate outputs)

### Analysis & Statistics
- **Visualization:** matplotlib, seaborn, plotly
- **Scientific Computing:** scipy
- **Machine Learning:** scikit-learn

## Secondary Frontend: Next.js Dashboard

### Frontend Stack
- **Framework:** Next.js 15 (App Router)
- **Language:** TypeScript
- **Runtime:** Node.js 20+
- **Package Manager:** pnpm

### Frontend Libraries
- **UI Framework:** React 19 (Server Components)
- **Styling:** Tailwind CSS
- **UI Components:** Radix UI, Lucide React
- **Data Visualization:** Recharts
- **File Parsing:** remark, gray-matter, csv-parse
- **Date Utilities:** date-fns
- **Form Handling:** React Hook Form

### Frontend Development
- **Linting:** ESLint
- **Type Checking:** TypeScript strict mode

## Integration Layer

### Data Flow
Python analysis scripts → `.pkl`, `.csv`, `.json` files → Next.js dashboard reads files → Recharts visualization

### File Format Standards
- **Intermediate data:** `.pkl` (pickle serialized objects)
- **Exported analysis:** `.csv` (for spreadsheets), `.json` (for web consumption)
- **Visualizations:** PNG (generated, not committed)

## Obsidian Integration

### Smart Connections Embeddings
- **Model:** TaylorAI/bge-micro-v2
- **Dimensions:** 384-dimensional vectors
- **Storage:** `.smart-env/multi/*.ajson` files

### Multi-Vault Setup
- **Journal Vault:** 2,600+ entries (1992-2025)
- **Notes Vault:** 5,500+ interconnected notes
- **Analysis Vault:** This repository with Python scripts

## Development Tools

### Code Editors
- **Recommended:** VS Code
- **Extensions:** Python, Pylance, ESLint, Prettier

### Version Control
- **Git:** For code, scripts, documentation
- **Ignored:** `.pkl`, `.csv`, generated data files, visualizations

### AI Development Partners
- **Claude Code:** Primary for iterative development (200k token context)
- **Gemini CLI:** Large-scale processing and bulk operations (1M-2M tokens)
- **NotebookLM:** Exploratory analysis and semantic search

---

## Framework-Specific Guidelines

### Python Data Science
See: `standards/backend/python-data-science.md` for comprehensive patterns

### Analysis Methods
See: `standards/backend/analysis-methods.md` for computational vs. interpretive distinction

### Next.js Dashboard
See: `standards/frontend/nextjs-dashboard.md` for integration with Python outputs

### Python Testing
See: `standards/testing/python-testing.md` for pytest patterns

---

## Development Commands

### Python Environment
```bash
# Install dependencies
pip install -r requirements.txt

# Run analysis scripts (numbered for execution order)
python Analysis/scripts/01_data_loader.py
python Analysis/scripts/02_eda.py
# ... etc
```

### Next.js Dashboard
```bash
cd dashboard
pnpm dev          # Start dev server
pnpm build        # Build for production
pnpm lint         # Run ESLint
```

### Full Pipeline
```bash
# Run all Python analysis
for script in Analysis/scripts/*.py; do python3 "$script"; done

# Then start dashboard to visualize results
cd dashboard && pnpm dev
```
