# Learning log

## 2026-11-20 — Session 0: setup
- **Learned:** a venv is a separate Python + packages just for this project; `.gitignore` keeps `data/` and `.venv/` out of git; the Keychain prompt wanted my Mac password, not a GitHub one.
- **Next:** choose question, subreddits and time window; download data from Arctic Shift.

## 2026-11-21 — Session 1: ML basics and data
- **Learned:** supervised learning needs labels (the thing I predict for a new example, which can be a number or a category); my project is unsupervised. In Python: reading a file line by line instead of loading it all, glob and file objects are lazy iterators, `Counter`, and that Jupyter remembers variables between cells (hidden state). Counts mean nothing without percentages.
- **Still confused:** picking the label in supervised examples; key vs value when looping over a dict; when Jupyter is using old variables.
- **Next:** read ~20 removed posts per `removed_by_category` and decide which to keep, then load the data into pandas.

## 2026-11-21 — Session 2: loading into pandas
- **Learned:** a DataFrame is a table of columns (Series) that share one index, and each column has one dtype (on pandas 3, text shows as `str` and missing values as `NaN`). I loaded all 171,756 posts by building a list of small dicts and calling `pd.DataFrame` once. `df = ...` replaces the whole table, so to change one column I write `df["col"] = ...`. Timestamp units matter: seconds vs milliseconds is the difference between 2024 and 1970.
- **Still confused:** `groupby` and how it splits the table; the pandas docs page was too dense (NumPy parts); haven't checked that the converted dates match session 1 yet.
- **Next:** Restart and Run All then step 4 (removed-category share per subreddit with `groupby` + `value_counts`).

## 2026-11-24 — Session 2, part 2: groupby and samples
- **Learned:** `groupby` splits the table into piles (one per subreddit or category), does the same thing to each pile, and puts the results together. `value_counts` with `dropna=False` keeps the not-removed posts (`NaN` = empty cell, a post that was never removed), and `normalize=True` gives shares that add up to 1 per subreddit. Filtering rows: a True/False column inside `df[...]` keeps the True rows, and `.sum()` on it counts them (True = 1). `== NaN` never matches, so I use `.isna()`. `sample(20)` fails on a pile smaller than 20, so I shuffle with `sample(frac=1)` and take `groupby(...).head(20)`; `groupby` also needs `dropna=False` to keep the `NaN` pile. Step 4 result: technology has the most moderator removals (~24%), artificial and ClaudeAI the most automod (~44% and ~43%).
- **Still confused:** writing the code myself without hints; it clicks when I go one small piece at a time. Got tired after ~2.5 hours.
- **Next (Friday, after Thursday's exam):** read the samples for each category (start with `NaN`), write one note per category, decide keep/drop, then build the `text` column.