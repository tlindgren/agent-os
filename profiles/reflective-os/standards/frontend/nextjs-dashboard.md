# Next.js Dashboard: Reflective OS

## Purpose & Architecture

The Next.js dashboard consumes Python analysis outputs and provides interactive visualization of:
- Sentiment trajectories over time
- Topic distribution and evolution
- Framework mapping (Fowler's Stages, etc.)
- Network analysis and relationships
- Cross-dimensional patterns

### Data Flow
```
Python Analysis → .pkl/.csv/.json files → Next.js Dashboard → Recharts Visualizations
```

## File Organization

```
dashboard/
├── app/
│   ├── layout.tsx           # Root layout with global styles
│   ├── page.tsx             # Dashboard home page
│   ├── api/                 # Data loading endpoints
│   │   ├── sentiment/route.ts
│   │   ├── topics/route.ts
│   │   └── ...
│   └── [analysis]/          # Dynamic routes for different analyses
├── components/
│   ├── ui/                  # Headless UI primitives
│   ├── charts/              # Chart components using Recharts
│   │   ├── SentimentChart.tsx
│   │   ├── TopicDistribution.tsx
│   │   └── ...
│   └── ...
├── lib/
│   ├── utils.ts
│   ├── data-loader.ts       # Load .pkl/.csv/.json files
│   └── ...
└── public/
    └── data/                # Symlink or copy of Analysis/data/
```

## Data Loading

### Consuming Analysis Outputs

**From `.csv` files (recommended for dashboards):**
```typescript typescript path=null start=null
import { parse } from 'csv-parse/sync';
import fs from 'fs';

export function loadSentimentData() {
  const content = fs.readFileSync('public/data/sentiment_by_year.csv', 'utf-8');
  const records = parse(content, { columns: true });
  return records.map(r => ({
    year: parseInt(r.year),
    sentiment: parseFloat(r.sentiment),
    count: parseInt(r.count)
  }));
}
```

**From `.json` files (for complex structures):**
```typescript typescript path=null start=null
export function loadTopics() {
  const data = require('public/data/topics.json');
  return data.topics.map(topic => ({
    id: topic.id,
    words: topic.top_words,
    weight: topic.weight
  }));
}
```

### File Access Pattern

**Keep data files synchronized:**
1. Python scripts save outputs to `Analysis/data/`
2. Dashboard accesses via `public/data/` symlink or copy
3. Use relative paths from `public/` directory

**Setup:**
```bash
cd dashboard
ln -s ../Analysis/data public/data
```

## Visualization with Recharts

### Chart Components

**Sentiment trajectory:**
```typescript typescript path=null start=null
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend } from 'recharts';

export function SentimentChart({ data }) {
  return (
    <LineChart width={800} height={400} data={data}>
      <CartesianGrid strokeDasharray="3 3" />
      <XAxis dataKey="year" />
      <YAxis label={{ value: 'Sentiment Score', angle: -90, position: 'insideLeft' }} />
      <Tooltip />
      <Legend />
      <Line type="monotone" dataKey="sentiment" stroke="#8884d8" name="Mean Sentiment" />
    </LineChart>
  );
}
```

**Topic distribution (stacked bar):**
```typescript typescript path=null start=null
import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, Legend } from 'recharts';

export function TopicDistribution({ data }) {
  return (
    <BarChart width={800} height={400} data={data}>
      <CartesianGrid strokeDasharray="3 3" />
      <XAxis dataKey="phase" />
      <YAxis />
      <Tooltip />
      <Legend />
      {topics.map(topic => (
        <Bar key={topic.id} dataKey={topic.name} stackId="topics" fill={topic.color} />
      ))}
    </BarChart>
  );
}
```

### Accessibility & Metadata

**Always include:**
- Descriptive chart titles
- Axis labels with units
- Data source attribution in caption
- Year range or date context
- Analysis method note

```typescript typescript path=null start=null
export function SentimentChart({ data }) {
  return (
    <div>
      <h2 className="text-xl font-bold mb-4">Sentiment Trajectory (1992-2025)</h2>
      <LineChart ...>
        {/* chart content */}
      </LineChart>
      <p className="text-sm text-gray-600 mt-2">
        Data: 2,616 journal entries | Method: VADER sentiment analysis | 
        Generated: {new Date().toLocaleDateString()}
      </p>
    </div>
  );
}
```

## Data Format Standards

### From Python: What to Expect

**`.csv` files for time series:**
```csv
year,sentiment,count,std
1992,0.52,28,0.15
1993,0.48,45,0.18
...
```

**`.json` files for complex data:**
```json
{
  "topics": [
    {
      "id": 0,
      "name": "Faith & Spirituality",
      "top_words": ["faith", "god", "believe", "spiritual"],
      "weight": 0.085
    }
  ]
}
```

### Dashboard Assumptions

Ensure Python outputs follow these patterns:
- Time series: sorted by date, with consistent column names
- Categories: use consistent IDs and names
- Numerical values: use `null` for missing, not empty strings
- Dates: ISO format (YYYY-MM-DD) or numeric year

## Performance Considerations

### Large Datasets

For 2,600+ entries:
- Load data at build time (static generation) when possible
- Use pagination or filtering for large visualizations
- Consider data aggregation (yearly vs. daily)
- Lazy load individual charts

```typescript typescript path=null start=null
// Load aggregated yearly data instead of per-entry data
export const dynamic = 'force-static';

export async function generateStaticParams() {
  return [{ slug: 'sentiment' }, { slug: 'topics' }];
}
```

### File Size Management

- `.csv` preferred over `.pkl` for web (pkl is Python-specific)
- `.json` for nested/complex structures
- Aggregate or summarize before exporting to web
- Consider `.gzip` for large files

## Integration with Python Pipeline

### Expected Workflow

1. **Python produces outputs:**
   ```bash
   python Analysis/scripts/05_sentiment_analysis.py
   # Saves: Analysis/data/sentiment_by_year.csv
   ```

2. **Dashboard reads outputs:**
   ```bash
   cd dashboard
   npm run dev  # Server reads from public/data/
   ```

3. **Visualizes results:**
   - Charts render automatically
   - User explores patterns
   - Reports generated from dashboard

### Keeping Data Fresh

**During development:**
```bash
# Terminal 1: Python analysis pipeline
for script in Analysis/scripts/*.py; do python3 "$script"; done

# Terminal 2: Dashboard dev server
cd dashboard && npm run dev

# Changes to Analysis/data/ automatically reload via Recharts
```

## Component Patterns

### Reusable Chart Component

```typescript typescript path=null start=null
interface ChartProps {
  title: string;
  data: any[];
  dataKey: string;
  description?: string;
  metadata?: string;
}

export function AnalysisChart({ title, data, dataKey, description, metadata }: ChartProps) {
  return (
    <div className="border rounded-lg p-6 bg-white">
      <h3 className="text-lg font-semibold mb-2">{title}</h3>
      {description && <p className="text-sm text-gray-600 mb-4">{description}</p>}
      
      <LineChart width={800} height={400} data={data}>
        {/* chart implementation */}
      </LineChart>
      
      {metadata && <p className="text-xs text-gray-500 mt-2">{metadata}</p>}
    </div>
  );
}
```

### Loading State

```typescript typescript path=null start=null
export function AnalysisDashboard() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function loadAnalysis() {
      try {
        const response = await fetch('/api/sentiment');
        const data = await response.json();
        setData(data);
      } catch (error) {
        console.error('Failed to load analysis:', error);
      } finally {
        setLoading(false);
      }
    }

    loadAnalysis();
  }, []);

  if (loading) return <div>Loading analysis...</div>;
  if (!data) return <div>No data available</div>;

  return <SentimentChart data={data} />;
}
```

## Deployment Considerations

### Vercel Deployment

Dashboard can be deployed independently:
```bash
cd dashboard
vercel deploy
```

**Environment:**
- Python analysis runs locally or on separate compute
- Dashboard accesses analysis outputs via file system or API
- Static generation works if data doesn't change frequently
- On-demand generation if analysis runs frequently

### Local Development

For iterative analysis:
1. Run Python scripts locally
2. Start Next.js dev server
3. Charts update as data changes
4. Use browser dev tools to inspect data flow

## Next Steps

When expanding dashboard:
1. Add new chart types as analysis expands
2. Create dashboard page for each analysis phase
3. Add filters and drilling down capabilities
4. Consider real-time updates if analysis becomes frequent
5. Document data schemas for future components

See: `standards/global/tech-stack.md` for full tech stack details
