# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A learning project for exploring the California Housing dataset and predicting house prices. It is at the exploration stage: the only code is `explore.ipynb`, which loads the data and prints `head()` and `describe()`. There are no Python modules, tests, or lint configuration yet.

## Environment

```bash
source .venv/bin/activate          # activate the virtual environment
pip install -r requirements.txt    # install pinned dependencies
pip freeze > requirements.txt      # re-pin after installing a new package
```

- `requirements.txt` is a full `pip freeze`, so it lists transitive dependencies too. The packages actually chosen are pandas, scikit-learn, matplotlib, seaborn and ipykernel.
- Only `ipykernel` is installed, not `jupyter` or `jupyterlab`, so the notebook is run through an editor's notebook support (such as VS Code) with `.venv` selected as the kernel. There is no `jupyter notebook` command available.
- `pytest` is not installed yet. Add it to the venv and re-pin before writing tests.

## Dataset

Loaded with `fetch_california_housing(as_frame=True)` from scikit-learn; the DataFrame is `data.frame`. The first call downloads the data and caches it in `~/scikit_learn_data`, so nothing is stored in this repository.

- 20,640 rows, each one a 1990 census block group (a neighbourhood), not an individual house.
- Target: `MedHouseVal`, the median house value in units of $100,000 (4.526 means $452,600).
- Features: `MedInc` (median income, in units of $10,000), `HouseAge`, `AveRooms`, `AveBedrms`, `Population`, `AveOccup`, `Latitude`, `Longitude`.
- `MedHouseVal` is capped at 5.00001 and `HouseAge` at 52, so those maximum values mean "this or higher". Account for the cap when modelling or evaluating.
