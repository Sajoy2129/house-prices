# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A learning project for exploring the California Housing dataset and predicting house prices. The code is two notebooks: `explore.ipynb` explores the data and `model.ipynb` trains and compares models. There are no Python modules, tests, or lint configuration yet.

## Notebooks

Both notebooks are written as reports in numbered sections, read top to bottom, and follow the same conventions:

- All imports live in the first code cell.
- Every code cell is preceded by a markdown cell that explains, for a beginner, what the code does and what to look for in the output, with new terms in bold. Question sections use the question as the heading and end the markdown cell with a `**Finding:**` line. Keep this pattern when adding sections.
- "What to look for" and "Finding" lines quote the actual numbers from the output, so run new code first and write the real values, and update them if a change alters the results.
- Cells share state from top to bottom, so a later cell only works after the earlier ones have run.
- Plots use the matplotlib object-oriented style (`fig, ax = plt.subplots(...)`, then `ax.` methods) with a title and axis labels that include units.
- Edit cells with the notebook tools instead of rewriting the file as text. The Edit tool refuses `.ipynb` files, and `explore.ipynb` stores plot outputs as embedded PNGs, so its raw JSON is large.

### `explore.ipynb`

Sections 1 to 5 get to know the data (load, `describe()`, `data.DESCR`, missing values, histogram of `MedHouseVal`). Sections 6 to 9 each answer one question about price (correlation, income scatter, map coloured by price, rooms scatter). Section 10 is a summary of findings; update it when a new section adds one.

`data` and `df` come from section 1, and section 3 sets `display.max_columns` to `None` for the rest of the notebook.

### `model.ipynb`

Sections 1 to 3 prepare the data: load, split columns into `X` (8 features) and `y` (`MedHouseVal`), then split rows 80/20 into `X_train`, `X_test`, `y_train`, `y_test` with `random_state=42`. Sections 4 to 6 each train one model and score it with mean absolute error (MAE) on the test set. Section 7 is a results table, section 8 checks for overfitting, and section 9 predicts the price for a made-up neighbourhood typed into a dictionary.

| Model | Variable | Test MAE | Training MAE |
|---|---|---|---|
| Baseline (training-set average) | `baseline_price` | 0.906 | n/a |
| Linear regression | `model` | 0.533 | 0.529 |
| Random forest (100 trees, `random_state=42`) | `forest` | 0.328 | 0.122 |

- MAE is in units of $100,000; multiply by `DOLLARS_PER_UNIT` (defined in section 4) for dollars. The scores are stored as `baseline_mae`, `linear_mae`, `train_mae` (linear regression's training MAE), `forest_mae` and `forest_train_mae`.
- The random forest is the best model but overfits (test MAE is 2.7 times its training MAE). Its settings have not been tuned yet.
- Models are retrained every time the notebook runs. Nothing is saved to disk.
- When adding a section, also update two cells that list the notebook's contents: the intro cell at the top (one line per section) and the section 1 markdown (which section first uses each import). Add a row to the section 7 table when a new model is trained.
- Fit on the training set only, and score on the test set only at the end, to avoid data leakage.

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
