# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A learning project for exploring the California Housing dataset and predicting house prices. It is at the exploration stage: the only code is `explore.ipynb`. There are no Python modules, tests, or lint configuration yet, and no model has been trained.

## Notebook

`explore.ipynb` is written as a tutorial in numbered sections. So far: 1. load the data, 2. `describe()`, 3. show all columns and print `data.DESCR`, 4. check for missing values, 5. histogram of `MedHouseVal`.

- Every code cell is preceded by a markdown cell that explains, for a beginner, what the code does and what to look for in the output, with new terms in bold. Keep this pattern when adding sections.
- Cells share state from top to bottom: `data` and `df` come from section 1, and section 3 sets `display.max_columns` to `None` for the rest of the notebook.
- Plots use the matplotlib object-oriented style (`fig, ax = plt.subplots(...)`, then `ax.` methods) with a title and axis labels that include units.
- The file stores plot outputs as embedded PNGs, so the raw JSON is large. Edit cells with the notebook tools instead of rewriting the file as text.

## Environment

```bash
source .venv/bin/activate          # activate the virtual environment
pip install -r requirements.txt    # install pinned dependencies
pip freeze > requirements.txt      # re-pin after installing a new package
```

- The venv uses Python 3.13. There is no project `.gitignore`: `.venv` is ignored by the `.gitignore` that `venv` creates inside it, so any other generated files (saved plots, model files, caches) need a `.gitignore` entry before they appear.
- `requirements.txt` is a full `pip freeze`, so it lists transitive dependencies too. The packages actually chosen are pandas, scikit-learn, matplotlib, seaborn and ipykernel.
- Only `ipykernel` is installed, not `jupyter` or `jupyterlab`, so the notebook is run through an editor's notebook support (such as VS Code) with `.venv` selected as the kernel. There is no `jupyter notebook` command available.
- `pytest` is not installed yet. Add it to the venv and re-pin before writing tests.

## Dataset

Loaded with `fetch_california_housing(as_frame=True)` from scikit-learn; the DataFrame is `data.frame`. The first call downloads the data and caches it in `~/scikit_learn_data`, so nothing is stored in this repository.

- 20,640 rows, each one a 1990 census block group (a neighbourhood), not an individual house.
- Target: `MedHouseVal`, the median house value in units of $100,000 (4.526 means $452,600).
- Features: `MedInc` (median income, in units of $10,000), `HouseAge`, `AveRooms`, `AveBedrms`, `Population`, `AveOccup`, `Latitude`, `Longitude`.
- No missing values in any column, so no imputation (filling in gaps) is needed.
- `MedHouseVal` is right-skewed, with a spike in the last histogram bin caused by the cap below.
- `MedHouseVal` is capped at 5.00001 and `HouseAge` at 52, so those maximum values mean "this or higher". Account for the cap when modelling or evaluating.
