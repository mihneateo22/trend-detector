# Learning log

## 2026-11-20 — Session 0: setup
- **Learned:** a venv is a separate Python + packages just for this project; `.gitignore` keeps `data/` and `.venv/` out of git; the Keychain prompt wanted my Mac password, not a GitHub one.
- **Next:** choose question, subreddits and time window; download data from Arctic Shift.

## 2026-11-21 — Session 1: ML basics and data
- **Learned:** supervised learning needs labels (the thing I predict for a new example, which can be a number or a category); my project is unsupervised. In Python: reading a file line by line instead of loading it all, glob and file objects are lazy iterators, `Counter`, and that Jupyter remembers variables between cells (hidden state). Counts mean nothing without percentages.
- **Still confused:** picking the label in supervised examples; key vs value when looping over a dict; when Jupyter is using old variables.
- **Next:** read ~20 removed posts per `removed_by_category` and decide which to keep, then load the data into pandas.
