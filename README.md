# Netflix Streaming Trends: A Statistical Analysis using R

> **A full-stack data science project demonstrating production-ready R workflows: from raw multi-source ingestion and tidy data pipelines to statistical modeling, clustering, and automated reporting.**

---

## Project Overview

This project addresses a core **business and research problem** for streaming and film: *How do content features, audience preferences, and commercial performance relate across platforms?* By integrating **Netflix catalog data** (movies and TV shows) with **IMDb** (ratings, votes, box office, budget, awards, and cast/crew), the analysis delivers:

- **Content structure insights** — genre, country, language, and release patterns for movies vs. TV.
- **IMDb-driven analysis** — rating and vote distributions, budget–gross relationships, and the impact of directors, actors, writers, and awards (Oscar/Golden Globe) on ratings and box office.
- **Cross-platform comparison** — TMDB (Netflix) popularity vs. IMDb ratings/votes; linear regression for rating prediction; **k-means clustering** for content segments; **association rules** for type/platform preference patterns.

Deliverables are **actionable recommendations** for content investment, release strategy, and platform-specific positioning, backed by reproducible code and clear visualizations.

---

## Key Technical Competencies

- **Data Wrangling via `tidyverse`** — `dplyr`, `tidyr`, and pipelines for joins, mutations, and long/wide reshaping.
- **Tidy Data Principles** — consistent use of one observation per row, normalized keys (`title_clean`, `release_year`) for merges.
- **High-Dimensional Visualization with `ggplot2`** — faceting, scales (`scales`, `viridis`), themes (`ggthemes`), and interactive plots (`plotly`) for distributions, trends, and scatter plots.
- **Regex and String Manipulation** — `stringr` for duration parsing (e.g. `"2h 30m"` → minutes), genre/country splitting, and `str_extract` for year/duration.
- **Date/Time Handling with `lubridate`** — parsing multi-format dates (`parse_date_time`), `year()`, `month()`, and quarter aggregation for seasonality.
- **Statistical Modeling** — **Linear regression** (`lm`) with **`caret`** for train/test split and metrics (RMSE, MAE, R²); LOESS smoothing for nonparametric trends.
- **Unsupervised Learning** — **k-means clustering** (`cluster`) on standardized rating, popularity, and box office; interpretation of cluster profiles.
- **Association Rules** — **`arules`** and **`arulesViz`** for pattern mining (e.g. genre/country/budget rules associated with higher IMDb vs. Netflix performance).
- **Exploratory Data Quality** — **`skimr`** for concise data overviews; duplicate/missing checks and type coercion.
- **Reproducible Reporting** — **R Markdown** with code folding, TOC, and multi-format output (HTML, PDF) for narrative and code in one artifact.

---

## Data Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  DATA INGESTION                                                              │
│  • Netflix movies & TV (CSV)  • IMDb combined movies (Kaggle, 2010–2025)     │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  CLEANING & WRANGLING                                                        │
│  • Duplicate/missing checks  • Date parsing (lubridate)  • Duration regex      │
│  • Genres/Country split & count  • Type conversion & rating bins            │
│  • IMDb: votes/budget/gross parsing, award flags, cast/director/writer cols  │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  FEATURE ENGINEERING & JOIN                                                   │
│  • title_clean + release_year as composite key                                │
│  • inner_join Netflix ↔ IMDb for cross-platform analysis                     │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  STATISTICAL MODELING & INSIGHT GENERATION                                    │
│  • EDA: distributions, time trends, seasonality (ggplot2 + scales)           │
│  • Regression: IMDb rating ~ popularity, duration, budget, genres (caret)    │
│  • Clustering: k-means on rating/popularity/gross (scale + cluster)          │
│  • Association rules: apriori for IMDb-higher vs Netflix-higher patterns     │
│  • Narrative conclusions and business recommendations in R Markdown         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Advanced Methodology

| Area | Implementation |
|------|----------------|
| **Custom parsing** | Duration from `"Nh Nm"` via `str_extract` and numeric conversion; vote strings with `"k"` suffix normalized to counts. |
| **Regex / string manipulation** | Genre and country lists split with `separate_rows` and `strsplit`; title normalization (`tolower`, `trimws`) for robust joins. |
| **Statistical inference** | **Regression**: OLS with `lm`; train/test via **`caret::createDataPartition`**; **`postResample`** for RMSE, MAE, R². **Hypothesis-oriented** comparison of Oscar/Golden Globe winners vs non-winners (box plots, median differences). |
| **Dimensionality & scaling** | Variables scaled before k-means; log transforms for budget/gross in visualizations and models. |
| **Rule-based feature creation** | Award flags (`imdb_oscar_winner`, `imdb_golden_globe_winner`) from text; budget/duration bins for association rules. |

---

## Performance & Reproducibility

- **R Markdown** drives the entire report: one source file produces HTML (with `theme: cosmo`, TOC, code folding) and PDF, keeping code and narrative in sync.
- **Modular structure**: distinct chunks for load, clean, EDA (A/B/C), modeling, and conclusions so sections can be re-run or extended independently.
- **Explicit data paths and encodings**: `fileEncoding = "UTF-8"` where needed; CSV reads with `stringsAsFactors = FALSE` for predictable behavior.
- **Reproducibility-ready**: project is structured for **environment management** (e.g. **`renv`**): all dependencies are standard CRAN packages (`tidyverse`, `caret`, `cluster`, `arules`, `ggplot2`, `lubridate`, `stringr`, `skimr`, etc.), so an `renv::init()` and `renv::snapshot()` workflow can lock versions for production or portfolio runs.

---

## Technical Skill Mapping Table

| Project feature | R / package(s) | Role |
|-----------------|----------------|------|
| Load and inspect CSVs | **base R** (`read.csv`), **skimr** | Ingestion and quick EDA |
| Reshape, filter, join, mutate | **dplyr**, **tidyr** (tidyverse) | Tidy pipelines and merges |
| Parse dates and compute year/quarter | **lubridate** | Time features and seasonality |
| Extract duration and clean text | **stringr** (regex, `str_extract`, `str_split`) | Parsing and normalization |
| Bar/line/box/scatter plots and themes | **ggplot2**, **scales**, **viridis**, **ggthemes** | High-dimensional visualization |
| Train/test split and regression metrics | **caret** (`createDataPartition`, `postResample`) | Modeling and evaluation |
| Linear model fitting | **stats** (`lm`) | Rating prediction |
| K-means clustering | **cluster** (`kmeans`) | Content segmentation |
| Association rules and visualization | **arules**, **arulesViz** (`apriori`, `inspect`, `plot`) | Pattern mining |
| Interactive plots | **plotly** | Optional interactivity |
| Document generation with code + narrative | **knitr**, **rmarkdown** | Automated reporting |

---

## Repository structure (high level)

```
├── Netflix Streaming Trends A Statistical Analysis using R.Rmd   # Main analysis & report
├── duration_cleaning.Rmd                                        # Focused duration-cleaning module
├── csv/                                                          # Input/output data (Netflix, IMDb)
├── README.md                                                     # This file
└── *.html / *.pdf                                                # Rendered outputs
```

---

## How to run

1. **R** (≥ 4.0) and **RStudio** (recommended).
2. Install dependencies (e.g. `tidyverse`, `caret`, `cluster`, `arules`, `arulesViz`, `ggplot2`, `lubridate`, `stringr`, `skimr`, `ggthemes`, `scales`, `viridis`, `plotly`). Optional: `renv::restore()` if an `renv.lock` is added.
3. Set working directory to the project root; ensure **csv/** contains the expected Netflix and IMDb CSVs (or adjust paths in the Rmd).
4. **Knit** `Netflix Streaming Trends A Statistical Analysis using R.Rmd` to regenerate the HTML/PDF report.

---

*Expert yet accessible: this project shows mastery of the R ecosystem for real-world data ingestion, cleaning, visualization, modeling, and reporting—ready to extend or adapt for production-style analytics.*
