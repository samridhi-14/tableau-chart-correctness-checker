# tableau-chart-correctness-checker

An automated pipeline for checking real, published Tableau Public dashboards against
three independent categories of chart-design correctness: chart type, aggregation,
and axis scale. Built as part of a Master's dissertation at Gisma University of
Applied Sciences.

## What it does

Most Tableau dashboards are published without any formal review of whether their
charts actually use the right chart type, a semantically valid aggregation, or a
non-misleading axis scale. This project checks for all three, directly from a
dashboard's native `.twbx` file, something existing visualization-linting tools
(e.g. VizLinter, Draco) don't support since they're built for Vega-Lite
specifications, not Tableau's own file format.

- **Module 1 — Chart Type Correctness:** 9 rules adapted from VizLinter, checking
  whether a chart's mark type (bar, line, pie, scatter, etc.) is structurally
  appropriate for its fields.
- **Module 2 — Aggregation Correctness:** 9 rules (M1–M9) checking whether a
  field's aggregation function is semantically valid given its type and role,
  plus a Decision Tree / Random Forest consistency check and an independent
  comparison against a large language model (Claude).
- **Module 3 — Axis and Scale Correctness:** Recovers real data values from each
  dashboard's bundled `.hyper` extract to detect misleading, non-zero-based axes,
  after confirming empirically that Tableau's XML retains no axis-range data at all.

## Dataset

The pipeline was evaluated against 299 real dashboards downloaded from Tableau
Public, spanning 8 business categories (education, financial, healthcare, hr,
marketing, operations, sales, supply_chain), totalling 5,306 worksheets.

Due to file size, the dataset is not stored directly in this repository.

📁 **Dataset (Google Drive):** [LINK TO DATASET](https://drive.google.com/file/d/1m6qzj9GJ3NKf2i4CGRhA66tvOhJc-yX6/view?usp=sharing)

## Results summary

| Module | Checked | Correct | Flagged |
|---|---|---|---|
| 1 — Chart Type | 1,654 charts | 62.8% | 37.2% |
| 2 — Aggregation | 1,654 charts | 90.0% | 10.0% |
| 3 — Axis/Scale | 1,512 axis fields (22.6% resolvable) | — | 6 confirmed issues |

## How to run

1. Open `Tableau_ChartType_Checker.ipynb` in Google Colab
2. Download the dataset from the Drive link above and place it in your own
   Google Drive as `My_Dashboards.zip`
3. Run all cells in order (Runtime → Run all)
4. Module 2's LLM comparison cell requires an Anthropic API key, entered when prompted

## Author

Samridhi Pnadey — Data Science, AI and Digital Business, Gisma University of Applied Sciences
