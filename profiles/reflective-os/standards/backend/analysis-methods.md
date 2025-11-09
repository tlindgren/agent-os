# Analysis Methods: Reflective OS

## Computational vs. Interpretive Distinction

### What Computation Can Reliably Show

**Safe to state directly:**
- Temporal patterns: "Entries increased 23% during this period"
- Statistical measures: "Mean sentiment score is 0.545"
- Correlations: "Strong correlation (r=0.73) between relationship language and positive sentiment"
- Frequencies: "Topic 14 appears in 11% of entries from 2009-2012"
- Distribution patterns: "71.7% of entries scored as very positive"

### What Requires Human Interpretation

**Must flag limitations:**
- Psychological states: "Low sentiment scores suggest negative mood, but human review needed to assess clinical depression"
- Causal claims: "These themes co-occur, but directionality requires theoretical interpretation"
- Framework application: "This language pattern suggests Fowler Stage 4, but framework application requires human judgment"
- Life quality assessment: "This data shows patterns; doesn't indicate whether outcomes were positive or negative"
- Spiritual/theological claims: "Computational method identifies language markers, not theological validity"

**Better framing patterns:**
- "Computational pattern suggests [X]. Human interpretation needed to assess [Y]."
- "This correlation could indicate [possibility A] or [possibility B]. Theoretical framework application required."
- "The data shows [pattern]. This might mean [interpretation], but [limitation prevents certainty]."

## Theoretical Framework Application

### When Applying Frameworks

**Remember:**
- Computational approximation ≠ theoretical interpretation
- Keywords suggest patterns, don't define experiences
- Frameworks are lenses, not truth claims
- Multiple frameworks may illuminate different aspects
- Human judgment required for framework application

### Example: Fowler's Stages of Faith

**What computation can do:**
- Identify keyword density: "Stage 4 language appears in X% of entries"
- Track evolution: "Stage 3 markers decrease over time; Stage 4 markers increase"
- Correlate with phases: "Stage 4 language most dense in 2009-2012 phase"

**What computation cannot do:**
- Determine actual faith stage (that requires theological/pastoral judgment)
- Assess spiritual validity or authenticity
- Understand lived experience of faith
- Evaluate quality of spiritual development

**How to present findings:**
```
This keyword-based approach identifies computational patterns consistent with 
Fowler's Stage 4 (Individuative-Reflective) faith development during the 
2009-2012 period. The pattern is suggestive but cannot definitively assess 
actual faith stage. Serves as pattern detector for further exploration, 
not theological evaluation.
```

### Known Framework Limitations

**Attachment Theory:**
- Can identify relationship language patterns
- Cannot assess actual attachment style from text alone
- Requires psychological assessment for proper classification

**McAdams Narrative Identity:**
- Can detect redemptive/contamination sequence language
- Cannot determine whether sequences reflect true narrative structure
- Interpretive analysis needed for narrative coherence

**Ignatian Discernment:**
- Can identify consolation/desolation language
- Cannot assess actual spiritual states or God presence
- Requires spiritual direction for genuine discernment

## Method-Specific Limitations

### Sentiment Analysis (VADER)

**What it does well:**
- Captures positive/negative valence in informal text
- Handles emoticons, slang, social media language
- Provides compound score (0-1 scale)

**Known limitations:**
- Misses sarcasm and irony
- Struggles with complex or nuanced emotions
- Optimized for social media, not literary/journal writing
- Cannot assess emotional intensity beyond score range

**Always note:**
"VADER sentiment scores provide a valence measure but may not capture sarcasm, complex emotions, or subtle mood shifts in journal writing."

### Topic Modeling (LDA)

**What it does well:**
- Identifies recurring word co-occurrence patterns
- Shows topic distribution across documents
- Provides latent semantic structure

**Known limitations:**
- Topic labels are human-assigned (algorithm finds word clusters)
- Number of topics must be pre-specified
- May split semantically similar topics or combine dissimilar ones
- Requires human interpretation for meaning

**Always note:**
"LDA identifies word co-occurrence patterns. Resulting topics are labeled by humans based on top words; labels are interpretive, not definitive categories."

### Named Entity Recognition (NER)

**What it does well:**
- Identifies mentions of people, places, organizations
- Can track entity frequency and co-occurrence

**Known limitations:**
- Misses relationships if not explicitly named
- Struggles with ambiguous references (pronouns, nicknames)
- May conflate entities with similar names
- Context-dependent accuracy

**Always note:**
"Named entity recognition may miss relationships if participants are not explicitly named or are referred to with pronouns."

### Network Analysis (Wiki Links)

**What it does well:**
- Shows connectivity and hub nodes
- Identifies clusters and bridges
- Reveals network structure

**Known limitations:**
- Presence of link ≠ semantic importance
- User may link for different reasons (contrast, hierarchy, causation)
- Missing links due to naming inconsistencies
- Direction and edge weight not always meaningful

**Always note:**
"Network analysis shows link structure; link presence doesn't imply semantic importance or causal relationship."

## Handling Unexpected Findings

### When Patterns Emerge That Seem Surprising

**Document and escalate:**
```python python path=null start=null
# Unexpected anomaly—flag for investigation
if sentiment_2005 < 0.3:
    print("WARNING: 2005 sentiment unusually low (0.3 vs typical 0.5+)")
    print("Possible causes:")
    print("- Life event during this period")
    print("- Change in journaling style or language")
    print("- Data quality issue or entry gap")
    print("RECOMMENDATION: Human review of 2005 entries needed")
```

**Never:**
- Assume computational finding proves a theory
- Make psychological claims from patterns alone
- Draw causal conclusions from correlations

**Always:**
- Document the unexpected finding
- List possible explanations (data, method, reality)
- Flag for human investigation
- Suggest next steps for verification

## Stating Limitations Explicitly

### Template for Method Limitations

```
[Method Name] has these computational limitations:
- [Limitation 1]: [How it affects results]
- [Limitation 2]: [How it affects results]

This means the results should be interpreted as:
[Qualified statement]

Confidence level: [High for detecting patterns / Medium for identifying themes / Low for definitive assessment]

Requires human review for: [What aspects need judgment]
```

### Example

```
VADER Sentiment Analysis limitations:
- Misses sarcasm and irony
- Optimized for social media language, not literary writing
- Cannot assess emotional intensity beyond valence

This means sentiment scores show positive/negative direction but may not capture 
subtle or complex emotional states expressed in journal entries.

Confidence level: High for detecting temporal sentiment trends; Medium for 
identifying individual emotional peaks; Low for clinical assessment.

Requires human review for: Complex emotional passages, sarcasm, emotional 
intensity assessment.
```

## Exploratory vs. Hypothesis-Testing

### This Project is Exploratory

**Exploratory research:**
- Questions guide the analysis, not tests of predetermined hypotheses
- Unexpected patterns are interesting, not contradictions
- Iterative: findings suggest new questions
- Success = generating insights and understanding, not proving theories

**In exploratory work:**
- ✅ "This pattern suggests we should investigate X"
- ✅ "We didn't expect this; let's explore why"
- ✅ "This opens a new line of inquiry"
- ❌ "This proves X"
- ❌ "This definitively shows Y"

### Documentation Reflects This

Flag exploratory nature in all outputs:
- "This analysis explores [question]"
- "Preliminary findings suggest [pattern]"
- "This pattern warrants further investigation"
- "These results are suggestive, not conclusive"

## Collaboration with Human Judgment

### Signal When to Involve Human Interpretation

In code or docstrings:

```python python path=null start=null
# COMPUTATIONAL: Safe to state
print(f"Correlation coefficient: {correlation:.2f}")

# INTERPRETIVE: Needs human judgment
print("This might suggest X because of...")  # Don't do this

# COLLABORATIVE: Flag for review
print("Computational finding: Strong correlation between themes A and B")
print("Possible interpretations:")
print("- A causes B")
print("- B causes A")
print("- Both caused by C")
print("Human judgment needed: Which interpretation fits the narrative?")
```

### When Writing Reports

Separate sections:
1. **Computational findings** (what the data shows)
2. **Possible interpretations** (what it might mean)
3. **Limitations** (what we can't conclude)
4. **Next steps** (how to investigate further)

This distinction keeps technical rigor while acknowledging interpretive uncertainty.
