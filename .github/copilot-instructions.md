<!-- Auto-generated guidance for AI coding agents working on this repo. -->
# Copilot instructions for market_inputs

Purpose: help an AI coding agent be immediately productive editing data-prep notebooks and inputs in this repository.

- Repository shape: this project is a small data-preparation repo built around Jupyter notebooks and tabular inputs.

Key files and directories
- `*.ipynb` — primary executable code. Notebooks of interest: `Market_Characterization.ipynb`, `Calibration_Process.ipynb`, `Avoided_Cost_Benefits_for_tests.ipynb`, `Adoption_Model_Prep.ipynb`.
- `input/` — canonical place for Excel/CSV inputs (examples: `00_template_utilty_forecast_disagg.xlsx`, `11_Avoided_Cost.xlsx`, `12_Loadshapes.xlsx`, `assembled_measures_new_AJ.xlsx`).
- `output/` — generated CSVs, notably `output/measure_costs_benefits.csv` which downstream notebooks read.

What matters (behavioral patterns discovered in code)
- Notebooks use `pandas` heavily to `read_excel`, `read_csv`, manipulate DataFrames and then `to_csv` to produce `output/measure_costs_benefits.csv`.
  - Example reads: `pd.read_excel("input/11_Avoided_Cost.xlsx")`, `pd.read_csv("input/assembled_measures.csv")` (note: repo contains `assembled_measures_new_AJ.xlsx`).
  - Example write: `df.to_csv("output/measure_costs_benefits.csv", index=False)` — downstream notebooks expect this file to exist.
- Many notebooks assume local relative paths (`input/` and `output/`) and Excel sheet names — preserve these when running or refactoring code.

Typical workflow (how humans run things)
- Data sources live in `input/`. Update inputs (usually `.xlsx`) and then run the notebooks that assemble and transform them.
- A common sequence:
  1. `Avoided_Cost_Benefits_for_tests.ipynb` — loads multiple `input/*.xlsx` files, computes avoided costs and benefits, then writes `output/measure_costs_benefits.csv`.
  2. `Adoption_Model_Prep.ipynb` — reads `output/measure_costs_benefits.csv` and performs model-prep steps.
  3. `Market_Characterization.ipynb` and `Calibration_Process.ipynb` — produce intermediate data artifacts (pickles, Excel) and consume template files under `input/`.

Agent guidance (concise, actionable)
- Prefer running or modifying notebooks as scripts only when necessary. For reproducible edits, convert a notebook cell into a small `.py` helper and run it from the notebook or as a testable script.
- Preserve relative paths when adding code or tests. Use `Path('input') / 'filename.xlsx'` if adding cross-platform code.
- When reading/writing inputs/outputs, match current conventions:
  - Use `pd.read_excel("input/<file>.xlsx", sheet_name=...)` for templates.
  - Write final assembled measures with `df.to_csv("output/measure_costs_benefits.csv", index=False)`.
- Avoid in-place, non-documented edits to notebooks. If you must change a notebook cell, add a short text cell describing the change and why.
- If you add new scripts, add them top-level (small `scripts/` or `tools/`) and reference them in a notebook via `%run` or import.

Repository-specific gotchas and notes
- The codebase contains examples where `.csv` is expected (`input/assembled_measures.csv`) but the repository provides `assembled_measures_new_AJ.xlsx`. Confirm which input is canonical before automated fixes.
- Notebooks create intermediate artifacts (`.pkl`, `.xlsx`) — do not delete these without ensuring a recreation pathway exists.

What to include in PRs from an AI agent
- Small, focused changes with a short PR description referencing the affected notebook and the `input/` files used for testing.
- If you modify how outputs are produced (file name, columns), update all notebooks that consume that output and add a brief run/test note in the PR.

Where to look for examples in this repo
- `Avoided_Cost_Benefits_for_tests.ipynb` — full ETL example that reads many `input/*.xlsx` files and writes `output/measure_costs_benefits.csv`.
- `Adoption_Model_Prep.ipynb` — shows how `output/measure_costs_benefits.csv` is consumed.
- `Calibration_Process.ipynb` — shows pattern of reading `input/00_template_utilty_forecast_disagg.xlsx` and producing disaggregated stock tables.

Questions to ask the maintainer (if uncertain)
- Which input file is authoritative for assembled measures: `assembled_measures.csv` or `assembled_measures_new_AJ.xlsx`?
- Preferred way to run notebooks (direct Jupyter, `nbconvert`, or converted Python scripts)?

If you need to run things locally
- Use a Python environment with `pandas` and `openpyxl`/`xlrd` installed. The notebooks use those libraries to read Excel files.

End of file.
