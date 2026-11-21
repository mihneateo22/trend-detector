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
