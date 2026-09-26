# Netflix Titles — Data Cleaning & Analysis

A data cleaning and exploratory analysis project using Netflix's public
title catalog (via Kaggle). The goal was to practice cleaning a
moderately messy real-world dataset — heavy on missing values and
multi-value columns rather than a single corrupted row — and answer a
set of analytical questions defined before touching the data.

## Dataset

- **Source:** [Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows) (Kaggle)
- **Size:** 8,807 rows × 12 columns
- **Columns:** show_id, type, title, director, cast, country, date_added,
  release_year, rating, duration, listed_in, description

## Questions This Project Answers

1. What is the ratio of Movies to TV Shows?
2. Which country has produced the most content?
3. Which genre (`listed_in`) is the most common?
4. What is the distribution of `rating` (content age rating)?
5. Which director has made the most titles?
6. Which actor appears in the most titles?
7. How has the number of titles added per year changed?
8. Which `release_year` has the most titles?
9. Has the Movie/TV Show ratio changed over the years?
10. Do different countries focus on different genres?

## Data Quality Issues Found & How They Were Handled

- **Missing values, two different treatments.** `date_added`, `rating`,
  and `duration` each had only a handful of missing rows (10, 4, 3 out
  of 8,807) — dropped. `director` (2,621 missing), `cast` (825), and
  `country` (829) were left as `NaN` rather than dropped, since removing
  them would have discarded a large share of the dataset.
- **Hidden whitespace.** Several `date_added` values had a leading space
  (e.g. `' March 31, 2017'`), which silently breaks a straight
  `pd.to_datetime()` conversion — caught with `.str.strip()` first.
- **Multi-value columns.** `cast`, `country`, and `listed_in` each hold
  multiple comma-separated values per row (a title can have several
  countries, actors, or genres). Answering questions about individual
  countries, actors, or genres required splitting these columns and
  exploding them into one value per row before counting.
- **A trailing-comma artifact.** A stray trailing comma in some
  `country` values produced an empty-string entry after splitting,
  which surfaced as a blank "country" in results — filtered out
  explicitly before grouping.

## Key Findings

- **Movies dominate the catalog:** 69.7% Movies vs. 30.3% TV Shows overall.
- **United States leads by a wide margin** (3,680 title-country credits),
  followed by India (1,046) and the United Kingdom (803).
- **International Movies** is the single most common genre tag (2,752),
  ahead of Dramas (2,426) and Comedies (1,674).
- **TV-MA** is the most common content rating (36.5%), followed by TV-14 (24.5%).
- Growth in titles added per year was steady from 2008 through 2019.
  **2020 shows a real dip**, plausibly tied to COVID-related production
  slowdowns; **2021's apparent drop is a data artifact**, not a real
  decline — this dataset was collected around September 2021, so that
  year is only ~9 months complete.
- **The Movie/TV Show split has been shifting toward TV Shows over time:**
  Movies made up ~74% of titles added in 2017 vs. only ~47% in 2021 —
  the first year TV Shows outnumbered Movies.
- **2018** is the single `release_year` with the most titles overall,
  and also the peak year for Movies specifically.
- Country and genre focus clearly differ — e.g. Zimbabwe's catalog
  leans International Movies, while other countries skew toward
  Documentaries or Dramas.

## Tools

Python, pandas, Jupyter Notebook

## Files

- `netflix-titles-cleaning.ipynb` — full cleaning and analysis notebook
- `netflix_titles.csv` — raw dataset
