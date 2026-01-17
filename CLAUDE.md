# CLAUDE.md - AI Assistant Guide for Chess Games Analysis

## Repository Overview

This repository contains a data science project analyzing chess games from Lichess.org. The primary focus is comparing performance metrics between evenly-matched games (where players have equal ratings) and uneven games (where players have different ratings).

**Repository**: Chess-Games-analysis
**Primary Language**: Python (Jupyter Notebook)
**Analysis Type**: Exploratory Data Analysis (EDA) of chess game statistics
**Dataset Size**: ~20,000 chess games

## Codebase Structure

```
Chess-Games-analysis/
├── Chess Data set analysis.ipynb   # Main analysis notebook (22 cells: 19 code, 3 markdown)
├── games.csv                        # Chess games dataset (20,059 games)
├── README.md                        # Basic project description
└── CLAUDE.md                        # This file - AI assistant guide
```

### Key Files

#### `Chess Data set analysis.ipynb`
The central analysis notebook containing:
- **19 code cells**: Data loading, transformation, visualization, and statistical analysis
- **3 markdown cells**: Commentary on findings
- **Analysis sections**:
  1. Data loading and initial exploration
  2. Opening strategy analysis (most common chess openings)
  3. Game length analysis (number of turns/moves)
  4. Victory status analysis (mate, resign, timeout, etc.)
  5. Even vs. uneven games comparison
  6. Rating-based segmentation and analysis

#### `games.csv`
Dataset with 20,059 chess games containing the following columns:
- `id`: Unique game identifier
- `rated`: Boolean indicating if game affects player ratings
- `created_at`, `last_move_at`: Timestamps
- `turns`: Number of moves in the game
- `victory_status`: How the game ended (mate, resign, outoftime, draw)
- `winner`: Which side won (white, black, or draw)
- `increment_code`: Time control format
- `white_id`, `black_id`: Player usernames
- `white_rating`, `black_rating`: Player ELO ratings
- `moves`: Full game notation
- `opening_eco`, `opening_name`, `opening_ply`: Opening classification

## Development Environment

### Required Libraries
```python
import pandas as pd           # Data manipulation
import numpy as np            # Numerical operations
import matplotlib.pyplot as plt  # Plotting
import seaborn as sns         # Statistical visualizations
from scipy import stats       # Statistical analysis
```

### Python Version
- Python 3.x (recommended: 3.8+)

### Installation
No `requirements.txt` exists. Install dependencies manually:
```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

## Key Analysis Patterns

### 1. Even vs. Uneven Games Classification
```python
even_games = (games['white_rating'] - games['black_rating'] == 0) |
             (games['black_rating'] - games['white_rating'] == 0)
evengames_data = games[even_games]
```

### 2. Opening Analysis
```python
opening_number = games['opening_name'].map(
    lambda n: n.split("|")[0].split(":")[0]
).value_counts().head(20)
```

### 3. Visualization Pattern
Most analyses use bar plots for categorical data:
```python
games['victory_status'].value_counts().plot.bar()
```

### 4. Data Binning for Continuous Variables
```python
pd.cut(games['turns'], 20, precision=0).value_counts().sort_index().plot.bar()
```

## Key Findings (as noted in notebook)

1. **Sicilian Defense** is the most common opening for Black
2. **Evenly matched games** tend to have more moves on average
3. **Rating parity** correlates with game length (more competitive = longer games)

## Development Workflow

### For AI Assistants Working on This Repository

#### Before Making Changes

1. **Read the notebook first**: Always examine existing analysis before proposing changes
2. **Understand the data schema**: Review `games.csv` structure and column meanings
3. **Check current branch**: You should be working on a `claude/*` prefixed branch

#### When Adding New Analysis

1. **Maintain consistency**: Follow the existing pattern:
   - Import statements at the top (cell 1)
   - One analysis per code cell
   - Add markdown cells to explain findings
   - Use similar visualization styles (bar plots, seaborn themes)

2. **Data integrity**: The notebook assumes a specific file path for `games.csv`:
   ```python
   # Current (Windows path - may need updating):
   games = pd.read_csv(r"C:\Users\User\Desktop\Chess Analysis\games.csv")

   # Recommended (relative path):
   games = pd.read_csv("games.csv")
   ```

3. **Avoid over-engineering**: This is an exploratory analysis project, not production code
   - No need for classes or complex abstractions
   - Keep analyses simple and readable
   - Focus on insights, not code architecture

#### When Refactoring

- **Preserve existing analyses**: Don't remove working code
- **Update paths**: If you see absolute paths, suggest relative paths
- **Maintain output**: Ensure visualizations still render correctly
- **Keep it simple**: This is a Jupyter notebook for exploration, not a software package

## Common Tasks

### Task: Add New Visualization
1. Read the notebook to understand existing plot styles
2. Use similar libraries (matplotlib/seaborn)
3. Add in a new code cell (don't modify existing analyses)
4. Add a markdown cell below to explain findings

### Task: Fix Path Issues
The notebook uses an absolute Windows path. To fix:
```python
# Change from:
games = pd.read_csv(r"C:\Users\User\Desktop\Chess Analysis\games.csv")

# To:
games = pd.read_csv("games.csv")
```

### Task: Add Statistical Tests
When comparing even vs. uneven games:
```python
from scipy import stats
# Example: t-test for average game length
even_turns = evengames_data['turns']
uneven_turns = games[~even_games]['turns']
t_stat, p_value = stats.ttest_ind(even_turns, uneven_turns)
```

### Task: Extend Dataset Analysis
If analyzing new aspects:
1. Check for missing values first: `games.isnull().sum()`
2. Understand the data type: `games.dtypes`
3. Check unique values for categoricals: `games['column'].unique()`
4. Follow existing pattern of value_counts() + plot.bar()

## Git Workflow

### Branch Strategy
- **Development branches**: `claude/*` prefix required (e.g., `claude/add-claude-documentation-pFUWx`)
- **Push command**: Always use `git push -u origin <branch-name>`
- **Branch naming**: Must start with `claude/` and end with matching session ID

### Commit Guidelines
1. **Descriptive messages**: Explain what analysis was added/modified
2. **Logical grouping**: Commit related changes together
3. **Examples**:
   - ✅ "Add opening diversity analysis for even vs uneven games"
   - ✅ "Fix CSV file path to use relative path"
   - ❌ "Update notebook"
   - ❌ "Changes"

### Pre-commit Checklist
- [ ] Notebook cells execute without errors
- [ ] All visualizations render correctly
- [ ] CSV path is relative (not absolute)
- [ ] New markdown cells explain findings
- [ ] No sensitive data added

## Code Conventions

### Naming
- **Variables**: Snake_case with descriptive names
  - `even_games`, `evengames_data`, `opening_number`
- **DataFrames**: Suffix with `_data` when creating subsets
  - `evengames_data`, `unevengames_data`

### Analysis Structure
1. **Filter/Transform**: Create boolean mask or transformed column
2. **Subset**: Extract relevant data
3. **Aggregate**: Use `value_counts()`, `groupby()`, or statistical functions
4. **Visualize**: Create plot (usually `.plot.bar()`)

### Example Pattern
```python
# 1. Filter
even_games = (games['white_rating'] - games['black_rating'] == 0)

# 2. Subset
evengames_data = games[even_games]

# 3. Aggregate
winner_counts = evengames_data['winner'].value_counts()

# 4. Visualize
winner_counts.plot.bar()
```

## Data Considerations

### Data Quality
- **Ratings**: Some games have placeholder rating (1500) for unrated players
- **Timestamps**: In scientific notation format (1.50421E+12)
- **Openings**: Hierarchical naming with "|" and ":" separators

### Missing Data Handling
The current analysis doesn't explicitly handle missing values. When adding new analyses:
```python
# Check for missing data
games.isnull().sum()

# Drop if necessary
clean_data = games.dropna(subset=['column_name'])
```

### Performance Notes
- Dataset is ~20K rows - fully loads into memory
- No need for chunking or optimization
- Pandas operations are sufficiently fast

## Troubleshooting

### Common Issues

**Issue**: `FileNotFoundError` when loading CSV
**Solution**: Update path to `games.csv` (relative path)

**Issue**: Plots not displaying in notebook
**Solution**: Ensure `%matplotlib inline` or use `plt.show()`

**Issue**: Import errors
**Solution**: Install missing packages with pip

**Issue**: Notebook won't run
**Solution**: Ensure Jupyter is installed: `pip install jupyter notebook`

## Best Practices for AI Assistants

### DO:
- ✅ Read existing code before suggesting changes
- ✅ Maintain the exploratory, notebook-based approach
- ✅ Add markdown cells to explain new findings
- ✅ Use consistent visualization styles
- ✅ Commit changes with clear messages
- ✅ Test that notebook cells execute in order

### DON'T:
- ❌ Remove existing working analyses
- ❌ Over-engineer solutions (no classes, complex abstractions)
- ❌ Create separate Python modules (keep in notebook)
- ❌ Modify data file (`games.csv`)
- ❌ Add dependencies without mentioning them
- ❌ Push to branches not starting with `claude/`

## Future Enhancement Ideas

If asked to extend the analysis, consider:

1. **Opening Success Analysis**: Win rates by opening type
2. **Rating Correlation**: How rating difference predicts game outcome
3. **Time Control Analysis**: Impact of increment_code on game quality
4. **Temporal Patterns**: Game outcomes over time
5. **Player Behavior**: Analysis of specific player patterns
6. **Advanced Statistics**: Regression models, hypothesis testing

## Questions to Ask Users

When working on new analysis:
- What specific insight are you looking for?
- Should this compare even vs. uneven games?
- What visualization type would be most useful?
- Do you want statistical testing or just descriptive stats?

## Version History

- **v1.0** (2026-01-17): Initial CLAUDE.md creation
  - Documented current repository state
  - 22-cell notebook with opening, victory, and even/uneven game analysis
  - 20,059 games from Lichess

---

*This document should be updated whenever significant changes are made to the repository structure, analysis approach, or data schema.*
