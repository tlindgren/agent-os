# Python Testing: Reflective OS

## Testing Framework: pytest

Use `pytest` for all Python testing:

```bash
pip install pytest pytest-cov
pytest Analysis/tests/  # Run all tests
pytest -v              # Verbose output
pytest --cov           # Show coverage
```

## Test Organization

```
Analysis/
├── scripts/            # Analysis scripts (01-07, etc.)
├── tests/              # Test files
│   ├── __init__.py
│   ├── conftest.py     # Shared fixtures
│   ├── test_data_loader.py
│   ├── test_preprocessing.py
│   ├── test_analysis.py
│   └── fixtures/       # Test data
│       ├── sample_journal.pkl
│       └── sample_entries.csv
└── data/               # Real outputs (git-ignored)
```

## Testing Principles

### Test Data & Fixtures

**Create small, deterministic test datasets:**

```python python path=null start=null
# tests/conftest.py
import pytest
import pandas as pd

@pytest.fixture
def sample_entries():
    """Sample journal entries for testing."""
    return pd.DataFrame({
        'date': pd.date_range('2020-01-01', periods=10),
        'text': ['Sample entry'] * 10,
        'sentiment': [0.5] * 10,
    })

@pytest.fixture
def sample_corpus():
    """Load test corpus (small subset of real data)."""
    return pd.read_pickle('tests/fixtures/sample_corpus.pkl')
```

### Unit Tests: Data Processing

Test individual functions with known inputs/outputs:

```python python path=null start=null
# tests/test_preprocessing.py
def test_sentiment_rolling_average(sample_entries):
    """Test rolling average calculation."""
    result = calculate_sentiment_rolling_average(sample_entries, window_days=3)
    
    assert 'sentiment_rolling' in result.columns
    assert len(result) == len(sample_entries)
    assert result['sentiment_rolling'].notna().all()

def test_sentiment_rolling_average_empty_input():
    """Test handling of empty input."""
    df = pd.DataFrame({'date': [], 'sentiment': []})
    result = calculate_sentiment_rolling_average(df)
    
    assert len(result) == 0
    assert 'sentiment_rolling' in result.columns
```

### Integration Tests: Pipeline Steps

Test data flow between scripts:

```python python path=null start=null
# tests/test_analysis.py
def test_data_loader_to_preprocessing(tmp_path):
    """Test data flows from loader to preprocessing."""
    # Run loader
    corpus = load_journal_entries('test_data/')
    
    # Run preprocessing
    preprocessed = preprocess_text(corpus)
    
    # Verify output
    assert len(preprocessed) > 0
    assert 'tokens' in preprocessed.columns
    assert 'stopwords' not in str(preprocessed)  # Stopwords removed
```

### Validation Tests: Data Quality

Test that outputs meet expected constraints:

```python python path=null start=null
# tests/test_validation.py
def test_sentiment_scores_in_valid_range(sample_corpus):
    """Sentiment scores should be between -1 and 1."""
    scores = sample_corpus['sentiment']
    
    assert scores.min() >= -1.0, "Sentiment below -1"
    assert scores.max() <= 1.0, "Sentiment above 1"

def test_dates_within_expected_range(sample_corpus):
    """Journal entries should be within expected time range."""
    dates = pd.to_datetime(sample_corpus['date'])
    
    assert dates.min() >= pd.Timestamp('1992-01-01')
    assert dates.max() <= pd.Timestamp.now()
    assert dates.is_monotonic_increasing, "Dates not sorted"
```

## Mocking Obsidian Data

For testing without needing full vault:

```python python path=null start=null
# tests/conftest.py
from unittest.mock import Mock, patch

@pytest.fixture
def mock_obsidian_vault():
    """Mock Obsidian Smart Connections embeddings."""
    return {
        'embeddings': {
            'note_1': [0.1, 0.2, 0.3, ...],  # 384 dims
            'note_2': [0.15, 0.22, 0.31, ...],
        },
        'metadata': {
            'model': 'TaylorAI/bge-micro-v2',
            'dimensions': 384
        }
    }

@pytest.fixture
def mock_journal_entries():
    """Mock journal entries from vault."""
    return {
        'entries': [
            {
                'date': '2020-01-15',
                'content': 'Sample journal entry',
                'faith_phase': 'Integration',
            },
            # ... more mock entries
        ]
    }
```

## Testing Analysis Methods

### Computational Correctness

Test that methods produce valid outputs:

```python python path=null start=null
def test_topic_modeling_produces_valid_topics(sample_corpus):
    """LDA should produce specified number of topics."""
    topics = perform_topic_modeling(sample_corpus, n_topics=15)
    
    assert len(topics) == 15
    assert all('words' in t for t in topics)
    assert all(len(t['words']) > 0 for t in topics)
    assert all(0 <= t['weight'] <= 1 for t in topics)

def test_framework_mapping_valid_stages(sample_corpus):
    """Framework mapping should assign valid stage numbers."""
    stages = map_fowler_stages(sample_corpus)
    
    assert all(s in [3, 4, 5] for s in stages['stage'])
    assert all(0 <= s <= 1 for s in stages['confidence'])
```

### Known Limitation Testing

Test that methods fail gracefully with known limitations:

```python python path=null start=null
def test_sentiment_handles_sarcasm():
    """Document that VADER struggles with sarcasm."""
    text = "Oh great, another amazing day"
    score = analyze_sentiment(text)
    
    # This should be negative but VADER might score positive
    # Document this known limitation
    pytest.mark.xfail(reason="VADER cannot detect sarcasm")
    assert score < 0.5  # This will be marked as expected failure
```

## Fixture Examples

### Sample Test Data Fixtures

```python python path=null start=null
# tests/fixtures/create_fixtures.py
import pandas as pd
import pickle

# Create minimal test corpus
test_corpus = pd.DataFrame({
    'date': pd.date_range('1992-01-01', periods=100, freq='M'),
    'text': [f'Entry {i}' for i in range(100)],
    'tokens': [['entry', str(i)] for i in range(100)],
})

# Save for reuse in tests
with open('tests/fixtures/sample_corpus.pkl', 'wb') as f:
    pickle.dump(test_corpus, f)

print("✓ Created test fixtures")
```

### CSV Test Data

```csv
date,text,sentiment,topic
1992-01-01,Sample entry,0.5,0
1992-02-01,Another entry,0.7,1
1992-03-01,Yet another,0.3,0
```

## CI/CD Testing

### Running Tests in Pipeline

```bash
# Lint Python files
flake8 Analysis/scripts/ --max-line-length=100

# Run tests with coverage
pytest Analysis/tests/ --cov=Analysis/scripts --cov-report=html

# Type check (if using type hints)
mypy Analysis/scripts/
```

### Pre-commit Hook

```bash
# .git/hooks/pre-commit
#!/bin/bash
pytest Analysis/tests/ || exit 1
```

## Edge Cases to Test

### Data Quality Issues

```python python path=null start=null
def test_handles_missing_data():
    """Scripts should handle null values gracefully."""
    df = pd.DataFrame({
        'date': [pd.Timestamp('2020-01-01'), None],
        'sentiment': [0.5, None]
    })
    
    result = preprocess_data(df)
    # Should not crash, handles nulls appropriately

def test_handles_duplicate_entries():
    """Duplicates should be detected or handled."""
    df = pd.concat([sample_df, sample_df])  # Duplicate
    
    result = remove_duplicates(df)
    assert len(result) == len(sample_df)

def test_handles_out_of_range_dates():
    """Invalid dates should be caught."""
    df = pd.DataFrame({
        'date': pd.date_range('1800-01-01', periods=10),  # Before 1992
    })
    
    with pytest.raises(ValueError, match="Dates before 1992"):
        validate_dates(df)
```

### Performance Tests

```python python path=null start=null
@pytest.mark.slow
def test_full_pipeline_performance(benchmark):
    """Full pipeline should run in reasonable time."""
    result = benchmark(run_full_analysis_pipeline)
    # Mark tests that take >1 second with @pytest.mark.slow
    # Run with: pytest -m "not slow" for fast tests
```

## Test Naming Conventions

**Function names describe what's being tested:**

```python python path=null start=null
# Good test names
test_sentiment_rolling_average_correct_window_size()
test_preprocessing_removes_stopwords()
test_framework_mapping_handles_ambiguous_language()

# Bad test names
test_function()
test_1()
test_it_works()
```

## Documentation in Tests

```python python path=null start=null
def test_sentiment_rolling_average_edge_case():
    """
    Test rolling average at data boundaries.
    
    Edge case: First few entries have min_periods=1, so rolling average
    may differ from later entries which have full window. This is intentional
    to preserve time series length.
    """
    df = pd.DataFrame({
        'date': pd.date_range('2020-01-01', periods=5),
        'sentiment': [0.5, 0.6, 0.7, 0.8, 0.9]
    })
    
    result = calculate_sentiment_rolling_average(df, window_days=3)
    
    # First entry has only 1 data point
    assert result['sentiment_rolling'].iloc[0] == 0.5
    # After window fills, uses all 3 points
    assert result['sentiment_rolling'].iloc[2] == (0.5 + 0.6 + 0.7) / 3
```

## When to Skip or Mark Tests

```python python path=null start=null
@pytest.mark.skip(reason="Not implemented yet")
def test_future_feature():
    pass

@pytest.mark.xfail(reason="Known limitation: VADER doesn't handle sarcasm")
def test_sarcasm_detection():
    pass

@pytest.mark.slow
def test_full_corpus_analysis():
    pass

# Run tests excluding slow tests:
# pytest -m "not slow"
```

## Continuous Improvement

Track test coverage:
```bash
pytest --cov=Analysis/scripts --cov-report=term-missing
```

Aim for:
- ✅ 80%+ coverage for data processing functions
- ✅ 70%+ coverage for analysis scripts
- ⚠️ Lower threshold for exploratory/research code
- 📝 Always document why untested areas are exploratory

See: `.claude/technical-context.md` for analysis infrastructure details
