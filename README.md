# House Prices

A learning project that explores the California Housing dataset and asks one question: what makes a neighbourhood's houses expensive?

The project is at the exploration stage. The analysis lives in [`explore.ipynb`](explore.ipynb), a notebook written as a report that reads top to bottom. No model has been trained yet.

## Findings so far

| Question | Finding |
|---|---|
| Which column is most linked to price? | Median income, with a correlation of 0.69 |
| Do richer areas have pricier houses? | Yes, in a wide rising band |
| Where are the expensive areas? | On the coast, around San Francisco and Los Angeles |
| Do more rooms mean a higher price? | Only slightly, with a correlation of 0.15 |

Two things to keep in mind for modelling:

- House values are capped at $500,000, so the most expensive areas are all recorded at the cap.
- Location matters a lot, but not in a straight line, so latitude and longitude show almost no correlation with price.

## The dataset

The data comes from the 1990 U.S. census and ships with scikit-learn. It has 20,640 rows, each one a census block group (a small neighbourhood), with 8 features and one target: the median house value in units of $100,000.

Nothing is stored in this repository. The notebook downloads the data the first time it runs and scikit-learn caches it in `~/scikit_learn_data`.

## Run it yourself

You need Python 3.13 and an editor that can run notebooks, such as VS Code with the Jupyter extension.

```bash
git clone https://github.com/Sajoy2129/house-prices.git
cd house-prices
python3 -m venv .venv              # create a virtual environment
source .venv/bin/activate          # activate it (macOS and Linux)
pip install -r requirements.txt    # install the pinned dependencies
```

Then open `explore.ipynb` in your editor, select `.venv` as the kernel, and choose **Run All**.

## Built with

- [pandas](https://pandas.pydata.org/) for working with tables
- [scikit-learn](https://scikit-learn.org/) for the dataset
- [matplotlib](https://matplotlib.org/) for plots

## Next steps

- Split the data into training and test sets
- Fit a linear regression as a baseline model
- Measure its error on the test set
