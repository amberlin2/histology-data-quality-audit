# Histology Data Quality Audit

**Amber Locasto | Python · pandas · Clinical Informatics**

A rule-based audit of synthetic Barrett’s esophagus surveillance reports. This project examines missing information, inconsistent report values, and workflow timestamp errors, then routes records for human review.

My experience working in GI histology informed the focus on traceable records and practical review decisions. Completed for IAN 630, Health Informatics.

## Project question

Can simple checks identify data quality problems that affect report review and laboratory workflow measurement?

## Data

The instructor-provided teaching dataset contains **158 report records from 80 fictional patients**. A patient may have multiple reports. Fields include report and patient identifiers, clinical report values, and timestamps from collection through final reporting.

This is synthetic tabular report data, not tissue images or a live laboratory information system feed. The data and injected-error answer key were supplied by the instructor; they are not my original data-generation work.

The instructor CSVs and generator are excluded from this public-upload package pending redistribution permission. Obtain the original authorized course files and place them in `data/` to reproduce the analysis. Do not substitute real patient records.

## Approach

1. Inspect identifiers, missing values, and category counts.
2. Apply seven documented rules without changing source values.
3. Produce a report-level review table.
4. Summarize receipt-to-report turnaround time using valid endpoints.
5. Compare selected checks against the injected-error catalog.
6. Demonstrate a follow-up review rule for an unexpected category with blank confirmation.

## Results

| Measure | Result |
|---|---:|
| Reports / patients | 158 / 80 |
| Unique reports flagged | 30 (18.99%) |
| Missing processing timestamps | 8 |
| Missing Prague C measurements | 6 |
| Unexpected dysplasia labels | 2 |
| Prague C greater than M | 3 |
| Processing after slide readiness | 3 |
| Missing confirmation detected by baseline rule | 7 of 8 documented issues |
| Confirmation True with no dysplasia | 2 |
| Median receipt-to-report time | 46.42 hours |

Counts overlap because one report can trigger multiple rules. Six of seven baseline checks found all documented issues they targeted, with no unmatched flags for those targeted issues. This is not an evaluation of every possible error.

### A useful limitation

The baseline confirmation rule recognized `low_grade` but not the unexpected label `LGD`, so it missed one documented missing-confirmation issue. A separate follow-up rule routes blank confirmation with an unexpected grade for review. It does not assume the category or confirmation requirement is correct.

That report was already flagged for its label, so the follow-up adds a review reason while the number of unique flagged reports remains 30. Because the revision followed answer-key review, it is not independent validation.

## Run locally

The analysis was checked with Python 3.12.14 and pandas 2.2.3. A Jupyter-compatible editor is required.

```bash
python -m venv .venv
```

Activate on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Then:

```bash
python -m pip install -r requirements.txt
python -m jupyterlab
```

Before running, add the two authorized CSVs described in [data/README.md](data/README.md). Open `histology_analysis_final.ipynb` from the repository root and run all cells in order. The notebook uses relative paths to `data/`. Saved outputs can be read without the CSVs, but rerunning requires them.

## Files

| File | Purpose |
|---|---|
| `histology_analysis_final.ipynb` | Analysis, saved outputs, interpretation, and limitations |
| `histology_systems_brief.md` | Proposed staff review workflow |
| `requirements.txt` | Notebook dependencies |
| `data/README.md` | Required input filenames and data provenance |
| `.gitignore` | Excludes environments, caches, and course CSVs |

## Limitations

The errors were deliberately injected, and the rules were developed with documentation of those patterns. Results cannot estimate real laboratory error rates. These checks do not establish clinical accuracy or physical specimen identity, and do not cover all measurement plausibility issues. Turnaround time includes waiting and cannot establish causes of delay or laboratory performance.

This is an educational audit, not a validated clinical system. Flags require authorized source review before any correction.

## Acknowledgments

Course data, error catalog, and dataset documentation: IAN 630 instructor-provided materials. Analysis and interpretation: Amber Locasto. AI assistance supported Python troubleshooting, notebook organization, and proofreading; the author is responsible for reviewing and understanding the work.
