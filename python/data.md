# 📘 CURSOR PROMPTS – Python Data (NumPy, Pandas, Matplotlib)

**Rules • Prompts • Debug cho Data Science & Data Engineering trong Cursor IDE**

> File này thuộc thư mục [python/](./README.md).

---

## Mục lục

- [1. Rules cho Data Projects](#1-rules-cho-data-projects)
- [2. NumPy Prompts](#2-numpy-prompts)
- [3. Pandas Prompts](#3-pandas-prompts)
- [4. Matplotlib / Seaborn / Plotly Prompts](#4-matplotlib--seaborn--plotly-prompts)
- [5. Data Engineering Prompts](#5-data-engineering-prompts)
- [6. Debug Prompts](#6-debug-prompts)
- [7. Best Practices & Anti-patterns](#7-best-practices--anti-patterns)

---

## 1. Rules cho Data Projects

### 1.1 File `.cursor/rules/data-science.mdc`

```
---
description: Rules for data science / data analysis Python files
globs: *.py, notebooks/*.ipynb
---

- Use type hints: np.ndarray, pd.DataFrame, pd.Series
- Use vectorized operations – NEVER iterate rows with for loops
- Use descriptive variable names (not df, df2, tmp)
- Document data transformations with comments
- Handle missing data explicitly (don't silently drop NaN)
- Use logging, not print() for scripts
- Pin library versions in requirements.txt
- Separate data loading, processing, visualization into functions
- Use pathlib.Path for file paths
- Set random seeds for reproducibility
- Use constants for magic numbers (column names, thresholds)
- Memory efficiency: use appropriate dtypes (category, int8, float32)
```

### 1.2 Full `.cursorrules` cho Data project

```txt
You are an expert Python data scientist/engineer with deep knowledge
of NumPy, Pandas, Matplotlib, Seaborn, and data pipeline best practices.

# Data Code Rules
- Vectorized operations only. No row-by-row iteration.
- Type hints for all function signatures (pd.DataFrame, np.ndarray, etc.).
- Descriptive variable names (not df, df2, tmp, x, y).
- Document each transformation step.
- Handle missing data explicitly.
- Use logging module, not print().
- Reproducibility: set random seeds, pin versions.
- Memory efficiency: optimize dtypes.
- Modular: separate load, clean, transform, analyze, visualize.

# Pandas Specific
- Use .loc/.iloc for indexing (not chained indexing).
- Avoid SettingWithCopyWarning: use .copy() or .assign().
- Use method chaining for readability.
- Use categorical dtype for low-cardinality string columns.
- Use query() for complex filtering conditions.

# NumPy Specific
- Use broadcasting instead of explicit loops.
- Specify dtype explicitly when creating arrays.
- Use np.vectorize only as last resort (it's not truly vectorized).

# Visualization
- Always label axes (xlabel, ylabel, title).
- Use consistent color scheme.
- Use appropriate chart type for data.
- Save figures as PNG/SVG with proper DPI.
- Use fig, ax pattern (not plt.plot directly).
```

---

## 2. NumPy Prompts

### 2.1 Tạo NumPy computation module

```text
@codebase

Create a NumPy computation module for [mô tả: signal processing, linear algebra, statistics, image operations].

Requirements:
- Vectorized operations (no Python loops)
- Type hints: np.ndarray with shape comments
- Docstrings with Args, Returns, Examples
- Broadcasting where applicable
- Handle edge cases (empty arrays, NaN, inf)
- Memory-efficient (avoid unnecessary copies)
- Unit tests with np.testing.assert_array_almost_equal
```

### 2.2 Optimize NumPy code

```text
@file [filepath]

Optimize this NumPy code:

Current issues: [slow, high memory, etc.]

Please:
- Replace Python loops with vectorized NumPy operations
- Use broadcasting instead of explicit expansion
- Use in-place operations (out= parameter) to reduce memory
- Use appropriate dtype (float32 vs float64)
- Use np.einsum for complex tensor operations
- Use np.memmap for very large arrays
- Show before/after timing and memory comparison
```

### 2.3 Linear algebra operations

```text
@codebase

Implement [mô tả: matrix decomposition, eigenvalue computation, least squares, etc.] using NumPy.

Requirements:
- Use np.linalg functions
- Handle numerical stability
- Type hints with shape annotations
- Validate input dimensions
- Handle edge cases (singular matrices, etc.)
- Performance: use BLAS/LAPACK-backed operations
```

### 2.4 Statistical functions

```text
@codebase

Create statistical analysis functions for [mô tả]:

Requirements:
- Descriptive stats (mean, median, std, percentiles)
- Hypothesis testing (t-test, chi-square, etc.)
- Confidence intervals
- Bootstrap resampling (if needed)
- Handle NaN values (np.nanmean, etc.)
- Vectorized across multiple columns/groups
- Type hints and docstrings
```

---

## 3. Pandas Prompts

### 3.1 Data loading & cleaning

```text
@codebase

Create data loading and cleaning pipeline for [data source]:

Requirements:
- Load from [CSV / Excel / JSON / SQL / API / Parquet]
- Specify dtypes on read (reduce memory)
- Handle missing values strategy: [drop / fill / interpolate]
- Data type conversion (dates, categories, numerics)
- Remove duplicates
- Validate data quality (assertions on shape, nulls, ranges)
- Logging for each step
- Return clean DataFrame with proper index
```

### 3.2 Data transformation / Feature engineering

```text
@codebase
@file [filepath]

Create data transformations for [mô tả]:

Requirements:
- Method chaining style (.pipe(), .assign(), .query())
- Vectorized operations only
- GroupBy aggregations if needed
- Window functions (rolling, expanding, ewm)
- Merge/join with other datasets
- Pivot / melt / stack / unstack if needed
- Create derived features
- Handle edge cases (empty groups, NaN in operations)
- Type hints: pd.DataFrame → pd.DataFrame
```

### 3.3 Exploratory Data Analysis (EDA)

```text
@codebase
@file [data file or description]

Perform comprehensive EDA:

Steps:
1. Data overview: shape, dtypes, head, describe, info
2. Missing value analysis (count, percentage, pattern)
3. Distribution analysis (histograms, box plots, KDE)
4. Correlation analysis (heatmap, pairplot)
5. Categorical analysis (value_counts, bar charts)
6. Outlier detection (IQR, z-score)
7. Time series patterns (if temporal data)
8. Summary of findings and recommendations

Output: Clean Python script with functions and visualizations.
```

### 3.4 Pandas optimization

```text
@file [filepath]

Optimize this Pandas code for performance and memory:

Please:
- Replace loops/apply with vectorized operations
- Use appropriate dtypes (category, int8, float32, etc.)
- Use query() for complex filters
- Use eval() for column operations on large DataFrames
- Use chunking (read_csv with chunksize) for large files
- Use Parquet instead of CSV for intermediate storage
- Minimize copies (.loc assignment, inplace where safe)
- Show memory usage before/after
- Show timing before/after
```

### 3.5 GroupBy & Aggregation

```text
@codebase

Create GroupBy aggregation for [mô tả]:

Requirements:
- Single or multi-level groupby
- Named aggregation (.agg(new_col=('col', 'func')))
- Custom aggregation functions
- Transform (return same-size DataFrame)
- Filter groups by condition
- Window functions within groups
- Handle empty groups
- Reset index for clean output
```

### 3.6 Merge & Join operations

```text
@codebase

Merge/join datasets for [mô tả]:

Requirements:
- Identify join type: inner/left/right/outer/cross
- Handle duplicate keys
- Validate merge (validate='one_to_many', indicator=True)
- Handle conflicting column names (suffixes)
- Performance: sorted merge keys, categorical keys
- Verify result shape (no unintended cartesian product)
```

### 3.7 Time series analysis

```text
@codebase

Create time series analysis for [mô tả data]:

Requirements:
- DatetimeIndex setup
- Resampling (hourly, daily, monthly aggregation)
- Rolling statistics (mean, std, min, max)
- Lag features
- Seasonal decomposition
- Missing timestamp handling (reindex + interpolate)
- Visualization with proper date formatting
```

### 3.8 Export & Reporting

```text
@codebase

Create data export/report for [mô tả]:

Requirements:
- Export to [Excel / CSV / Parquet / JSON / HTML]
- Multiple sheets (Excel with openpyxl)
- Formatting (number format, column widths, headers)
- Summary statistics sheet
- Charts embedded in Excel (if needed)
- Automated filename with timestamp
- Compression for large files
```

---

## 4. Matplotlib / Seaborn / Plotly Prompts

### 4.1 Matplotlib chart

```text
@codebase

Create a [line / bar / scatter / histogram / heatmap / pie / subplot] chart:

Data: [mô tả data hoặc @file]

Requirements:
- Use fig, ax = plt.subplots() pattern
- Labeled axes (xlabel, ylabel, title)
- Legend if multiple series
- Grid if helpful
- Color scheme: [viridis / plasma / custom]
- Proper figure size and DPI
- Save as PNG/SVG
- Annotations/highlights if needed
- Tight layout (fig.tight_layout())
```

### 4.2 Multi-panel dashboard

```text
@codebase

Create a multi-panel visualization dashboard for [mô tả]:

Requirements:
- plt.subplots with grid layout (e.g., 2x3)
- Different chart types per panel
- Shared axes where appropriate
- Consistent style/colors across panels
- Suptitle for overall title
- Tight layout, proper spacing
- High-resolution output (dpi=150+)
- Color-blind friendly palette
```

### 4.3 Seaborn statistical visualization

```text
@codebase

Create Seaborn visualizations for [dataset]:

Charts needed:
- Distribution: histplot, kdeplot, ecdfplot
- Relationships: scatterplot, regplot, jointplot
- Categorical: boxplot, violinplot, swarmplot, barplot
- Heatmap: correlation matrix
- Pairplot for overview

Requirements:
- Consistent theme (sns.set_theme)
- Proper figure sizes
- Statistical annotations (p-values, CI)
- Save all figures
```

### 4.4 Plotly interactive chart

```text
@codebase

Create interactive Plotly chart for [mô tả]:

Requirements:
- Use plotly.express or plotly.graph_objects
- Hover tooltips with relevant info
- Zoom/pan/select capabilities
- Color scale / legend
- Responsive layout
- Export as HTML (standalone)
- Dashboard with multiple charts (Plotly Dash if needed)
```

### 4.5 Custom visualization style

```text
@codebase

Create a custom visualization style/theme for the project:

Requirements:
- Matplotlib rcParams or style sheet (.mplstyle)
- Consistent fonts, colors, line widths
- Color palette (primary, secondary, categorical, sequential)
- Figure sizes for paper/presentation/web
- Helper functions for common chart types
- Reusable across all project visualizations
```

---

## 5. Data Engineering Prompts

### 5.1 ETL Pipeline

```text
@codebase

Create an ETL pipeline for [mô tả: data source → transformations → destination]:

Requirements:
- Extract: [CSV / API / Database / S3 / FTP]
- Transform: cleaning, dedup, type conversion, feature engineering
- Load: [Database / Data warehouse / Parquet / CSV]
- Error handling with retry
- Logging each step
- Incremental processing (last run timestamp)
- Data validation checkpoints
- Idempotent (safe to re-run)
- Configuration-driven (YAML/JSON config)
```

### 5.2 Data validation

```text
@codebase

Create data validation for [dataset / pipeline step]:

Requirements:
- Use [pandera / great_expectations / custom]
- Schema validation (columns, dtypes, nullable)
- Value range checks
- Uniqueness constraints
- Referential integrity
- Custom business rules
- Report validation failures
- Halt or warn based on severity
```

### 5.3 Database operations

```text
@codebase

Create database utility for [mô tả]:

Requirements:
- Use [SQLAlchemy / psycopg2 / sqlite3]
- Connection management (context manager)
- Parameterized queries (prevent SQL injection)
- Bulk insert optimization
- Upsert support
- Transaction management
- Connection pooling
- Type hints for query results
```

### 5.4 Scheduled data job

```text
@codebase

Create a scheduled data job for [mô tả: daily report, sync, aggregation]:

Requirements:
- Use [APScheduler / Celery / cron + script]
- Idempotent execution
- Error handling with alerting
- Logging (start, progress, end, errors)
- Lock mechanism (prevent concurrent runs)
- Configuration (schedule, parameters)
- Health monitoring
- Retry on failure
```

---

## 6. Debug Prompts

### 6.1 Pandas SettingWithCopyWarning

```text
@file [filepath]

SettingWithCopyWarning in Pandas:
[paste warning]

Please:
- Identify the chained indexing issue
- Fix with .loc[], .copy(), or .assign()
- Explain why the original code was problematic
```

### 6.2 Memory Error / OOM

```text
@codebase

Out of memory when processing data:
[mô tả: file size, operations]

Please:
- Use chunked reading (pd.read_csv chunksize)
- Optimize dtypes (category, int8, float32)
- Process in chunks and aggregate
- Use Dask or Polars for out-of-core processing
- Use generators instead of lists
- Delete intermediate DataFrames (del + gc.collect)
```

### 6.3 Wrong results / Data mismatch

```text
@file [filepath]

Data transformation producing wrong results:
[mô tả expected vs actual]

Please:
- Add intermediate .shape / .head() / .describe() checks
- Check merge/join for duplicates (cartesian product)
- Check groupby for unexpected groups
- Check NaN handling in aggregations
- Verify column names and dtypes
- Add assertions for data quality
```

### 6.4 Slow Pandas operation

```text
@file [filepath]

This Pandas operation is slow:
[paste code or describe]

DataFrame shape: [rows x cols]

Please:
- Profile with %%timeit or cProfile
- Replace apply() with vectorized operation
- Use categorical dtype for string groupby
- Use numba/numpy for custom operations
- Consider Polars if significantly faster
- Show timing before/after
```

### 6.5 Matplotlib rendering issue

```text
@file [filepath]

Matplotlib chart issue:
[mô tả: overlapping labels, wrong scale, missing data, legend cutoff]

Please:
- Fix layout (tight_layout, constrained_layout)
- Fix tick formatting
- Adjust figure size
- Fix data mapping
- Add proper handling for NaN/inf in plot data
```

---

## 7. Best Practices & Anti-patterns

### ✅ DO

- Dùng vectorized operations (không for loop trên rows)
- Dùng `.loc[]` / `.iloc[]` cho indexing
- Dùng `.assign()` cho thêm columns (method chaining)
- Dùng `pd.Categorical` cho low-cardinality strings
- Dùng `fig, ax = plt.subplots()` pattern
- Dùng descriptive variable names (`sales_df`, not `df2`)
- Dùng `Parquet` thay vì CSV cho intermediate data
- Set random seeds cho reproducibility
- Validate data shape/types sau mỗi transformation
- Document mọi data assumption

### ❌ DON'T

- Không iterate rows với `for i, row in df.iterrows()`
- Không dùng chained indexing (`df[cond]['col'] = val`)
- Không dùng `inplace=True` (deprecated mindset, use assignment)
- Không dùng `plt.plot()` trực tiếp (dùng `ax.plot()`)
- Không forget label axes
- Không load toàn bộ large file vào memory
- Không dùng `float64` khi `float32` đủ
- Không ignore NaN – handle explicitly
- Không hardcode column names – dùng constants
- Không mix Pandas + for loops

---

> 📌 Quay lại [Python Core](./README.md) | [Handbook chính](../cursor-ide-handbook.md) | Xem thêm: [Web](./web.md) | [AI](./ai.md)
