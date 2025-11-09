# Coding Style: Reflective OS

## Python Conventions

### Naming
- **Variables & Functions:** `snake_case` (e.g., `sentiment_scores`, `calculate_rolling_average`)
- **Classes:** `PascalCase` (e.g., `SentimentAnalyzer`, `TopicModel`)
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_TOPICS`, `DEFAULT_WINDOW_SIZE`)
- **Meaningful names:** `faith_stage_indicators` not `fsi`, `corpus` not `data`, `tokens` not `t`

### Code Structure

**Functions:**
- Keep focused and single-purpose
- Extract repeated logic into utility functions
- Use meaningful function names that describe what they do
- Include docstrings for non-obvious functions

**Example (good):**
```python python path=null start=null
def calculate_sentiment_rolling_average(df, window_days=30):
    """Calculate rolling average of sentiment scores.

    Args:
        df: DataFrame with 'date' and 'sentiment' columns
        window_days: Size of rolling window (default 30)

    Returns:
        DataFrame with added 'sentiment_rolling' column
    """
    df = df.sort_values('date')
    df['sentiment_rolling'] = df['sentiment'].rolling(
        window=window_days,
        min_periods=1
    ).mean()
    return df
```

**Example (bad):**
```python python path=null start=null
def process(d, w=30):  # What does this do?
    d = d.sort_values('date')
    d['sr'] = d['s'].rolling(window=w, min_periods=1).mean()
    return d
```

### Comments & Documentation

**Self-documenting code (preferred):**
- Write code that explains itself through structure and naming
- Comments only for non-obvious "why", not "what"

**When to comment:**
- Explain preprocessing decisions: why filter this way?
- Note computational limitations
- Flag areas requiring human interpretation
- Document assumptions about data format

**Example:**
```python python path=null start=null
# Remove entries with missing sentiment scores
# (keeps chronological order for time series analysis)
df = df.dropna(subset=['sentiment'])

# Note: VADER sentiment may miss sarcasm and complex emotions
sentiment_scores = analyzer.polarity_scores(text)
```

### File Naming

**Python scripts:**
- Format: `01_description.py` (numbered for execution order)
- Example: `05_sentiment_analysis.py`, `06_framework_mapping.py`

**Data files:**
- Format: `name_description.extension`
- Be descriptive about content
- Example: `corpus_with_sentiment.pkl`, `sentiment_by_year.csv`

**Reports:**
- Format: `name-description.md`
- Use kebab-case
- Example: `phase1-exploration.md`, `depression-analysis.md`

**Visualizations:**
- Format: `name_description.png`
- Use snake_case
- Example: `sentiment_trajectory.png`, `topic_distribution.png`

## Next.js/TypeScript Conventions

### File Organization
```
dashboard/
├── app/              # Next.js App Router
├── components/       # React components
│   ├── ui/          # UI primitives
│   └── ...          # Custom components
├── lib/             # Utilities
├── public/          # Static assets
└── ...
```

### Naming
- **Components:** `PascalCase.tsx` (e.g., `SentimentChart.tsx`)
- **Utils/Hooks:** `camelCase.ts` (e.g., `useChartData.ts`, `formatDate.ts`)

### TypeScript
- Always use strict mode
- Define types for props, states, returns
- Avoid `any` type

---

## General Principles

### Clarity Over Cleverness
- Prefer readable code over optimized code
- Use descriptive variable names
- Structure functions logically
- Make dependencies explicit

### Documentation as You Code
- Document preprocessing decisions
- Explain non-obvious choices
- Note computational method limitations
- Flag areas requiring human interpretation

### Prepare for Future Readers
- Assume the next person reading this (including future you) is unfamiliar
- Use comments to bridge gaps between code and intention
- Include examples in docstrings when behavior isn't obvious

---

## Cross-Language Consistency

**Python ↔ TypeScript Coordination:**
- Python generates data files (`.pkl`, `.csv`, `.json`)
- TypeScript imports and consumes these files
- Use consistent naming across both: if Python creates `sentiment_by_year.csv`, TypeScript refers to it by that name
- Document data schema in comments when format isn't self-obvious
