# Python Data Science: Reflective OS

## Data Handling Best Practices

### Validation Before Processing
Always validate data before running analysis:

```python python path=null start=null
# Validate input data
assert 'entry_date' in df.columns, "Missing entry_date column"
assert df['entry_date'].notna().all(), "Null dates found"
assert df['entry_date'].min() >= pd.Timestamp('1992-01-01'), "Dates before 1992"
assert df['entry_date'].max() <= pd.Timestamp('2025-12-31'), "Future dates found"
```

**Check for:**
- Required columns present
- No unexpected null values
- Data types as expected
- Date ranges reasonable
- Numerical values in expected range

### Preprocessing Documentation

**Always document:**
- Why you're making each preprocessing decision
- What data was removed and why
- Any assumptions about data format
- Known limitations of the method

**Example:**
```python python path=null start=null
# Remove entries with missing sentiment scores
# (maintains chronological order for time series analysis)
df = df.dropna(subset=['sentiment'])
print(f"Removed {removed_count} entries with missing sentiment")

# Note: VADER sentiment analysis has known limitations:
# - May miss sarcasm and irony
# - Optimized for social media language
# - Requires human interpretation for complex emotions
```

### Intermediate Data Persistence

Save intermediate outputs to avoid re-running expensive operations:

```python python path=null start=null
import pickle

# After data loading and preprocessing
with open('Analysis/data/corpus_preprocessed.pkl', 'wb') as f:
    pickle.dump(preprocessed_data, f)

# Load in later script
with open('Analysis/data/corpus_preprocessed.pkl', 'rb') as f:
    preprocessed_data = pickle.load(f)
```

**Benefits:**
- Faster iteration during development
- Can re-run downstream analysis without reprocessing
- Preserves exact state between script runs
- Prevents data drift from re-computation

## File Output Standards

### Pickle Files (.pkl)
- **Use for:** Intermediate analysis results, preprocessed data
- **Not for:** Final deliverables (use CSV or JSON instead)
- **Why:** Preserves exact Python data structures between runs
- **Example:** `corpus_with_sentiment.pkl`, `document_topics.pkl`

### CSV Files (.csv)
- **Use for:** Tabular data meant for spreadsheets or R
- **Include:** Column headers, clear naming
- **Example:** `sentiment_by_year.csv`, `topic_distribution.csv`

### JSON Files (.json)
- **Use for:** Web consumption, structured data
- **Include:** Schema comments when format isn't obvious
- **Example:** `topics.json` (LDA topic words), `analysis_summary.json`

### PNG Visualizations
- **Generate but don't commit:** Add to `.gitignore`
- **Save with metadata:** Include title, data source, date
- **Example:** `sentiment_trajectory.png`, `topic_heatmap.png`

## Visualization Best Practices

### Metadata in Visualizations
Include essential information in every chart:

```python python path=null start=null
plt.title('Sentiment Trajectory (1992-2025)', fontsize=14, fontweight='bold')
plt.xlabel('Year')
plt.ylabel('Sentiment Score')
plt.figtext(0.01, 0.01, 'Data: 2,616 journal entries | Method: VADER sentiment analysis', 
            fontsize=8, style='italic', color='gray')
plt.tight_layout()
plt.savefig('Analysis/visualizations/sentiment_trajectory.png', dpi=150)
```

**Always include:**
- Descriptive title
- Axis labels with units
- Data source and sample size
- Analysis method used
- Date of generation (in metadata or caption)

### Accessible Visualizations
- Use colorblind-friendly palettes
- Don't rely on color alone to convey information
- Include clear legends
- Make fonts readable at normal viewing size

## Script Organization

### Numbered Execution Order
Number scripts for clear pipeline:
```
Analysis/scripts/
├── 01_data_loader.py
├── 02_eda.py
├── 03_preprocessing.py
├── 04_topic_modeling.py
├── 05_sentiment_analysis.py
├── 06_framework_mapping.py
└── 07_visualization.py
```

**Each script:**
- Has single responsibility
- Loads its required inputs
- Saves outputs for next script
- Includes validation checks
- Documents decisions in comments

### Full Pipeline Execution
```bash
# Run complete analysis pipeline
for script in Analysis/scripts/*.py; do python3 "$script"; done
```

## Library-Specific Patterns

### Pandas DataFrames
- Sort by date for time series: `df.sort_values('date')`
- Use `.copy()` when modifying to avoid SettingWithCopyWarning
- Document column meanings in code comments
- Use descriptive column names: `sentiment_score` not `s`

### NLTK & Text Processing
- Load stopwords once and reuse: `from nltk.corpus import stopwords`
- Document language choice for lemmatization
- Keep original text separate from processed tokens
- Save preprocessing config for reproducibility

### Scikit-learn Models
- Document hyperparameters and why chosen
- Save model objects with pickle if reusing
- Include cross-validation metrics
- Note any data preprocessing applied before modeling

### NetworkX Graph Analysis
- Keep nodes and edges clearly documented
- Export graph data to `.gexf` or `.json` for visualization tools
- Include node attributes (e.g., node importance scores)
- Document graph construction method

## Error Handling & Robustness

```python python path=null start=null
# Graceful error handling with informative messages
try:
    df = pd.read_csv('Analysis/data/corpus.csv')
except FileNotFoundError:
    raise FileNotFoundError(
        "Corpus file not found. Run 01_data_loader.py first to generate Analysis/data/corpus.csv"
    )

# Log warnings for data quality issues
if df['sentiment'].isna().sum() > 0:
    print(f"Warning: {df['sentiment'].isna().sum()} entries have missing sentiment scores")
```

## Performance Considerations

### Memory Efficiency
- Process large datasets in chunks if needed
- Use appropriate data types: `pd.Int64` for integers, not `float64`
- Delete intermediate DataFrames when no longer needed
- Monitor memory usage for large corpora

### Computation Time
- Save intermediate results to avoid re-computation
- Test on small dataset subset before running full pipeline
- Include timing information in logs
- Document expected runtime for each script

---

## When to Escalate to AI Assistant

Flag these situations in code comments or docstrings:
- **Computational approximation ≠ theoretical validity**
  - "VADER sentiment may not capture complex emotions"
  - "LDA topic modeling is pattern detection, not definitive interpretation"

- **Unexpected patterns requiring investigation**
  - "Anomalous spike in entries during 2005—verify data completeness"
  - "Topic distribution differs from expected—check preprocessing"

- **Methods with known limitations**
  - "Named entity recognition may miss relationships if not explicitly named"
  - "Temporal boundaries are user-defined, not data-derived"
