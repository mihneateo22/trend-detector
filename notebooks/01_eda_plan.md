# 01_eda plan
 
All of week 1 lives in `notebooks/01_eda.ipynb`. Each session starts with a markdown heading cell.
 
---
 
# Session 1: inspecting the raw files (done)
 
Goal: look inside the raw files before changing anything. No cleaning and no pandas yet.
 
- **Cell 1, setup:** imports and a sorted list of raw files from `Path("..") / "data" / "raw"` and `.glob("*.jsonl")`.
- **Cell 2, counts and date range:** per file, count posts and track min/max `created_utc`. Dates run 2024-11-01 → 2025-12-30 (end date exclusive, noted in README).
- **Cell 3, field names:** first post only, `fh.readline()` + `json.loads`, print the keys.
- **Cell 4, odd values:** per file, count empty `selftext`, `[removed]`/`[deleted]` selftext, `[deleted]` author, as counts and percentages.
- **Cell 5, findings:** markdown bullets.
- Wrap-up: LOG.md entry ✔, git commit ✔
---
 
# Session 2: removed posts and loading into pandas
 
Goal: get all 171,756 posts into one pandas DataFrame, then use it to decide which removed posts to keep. ~2.5 hours. Stop at the wrap-up even if something is unfinished.
 
## Step 1: learn the pandas mental model (30 min)
 
Read:
- pandas User Guide, **"Intro to data structures"**: the Series and DataFrame sections only. This explains what a table is in pandas.
- *Python Data Science Handbook*, ch. 3, **"Introducing Pandas Objects"**: same ideas, with small examples.
Be able to answer in your own words:
1. What is a Series? (values + an index + one dtype)
2. What is a DataFrame? (Series sharing one index, one per column)
3. What is a dtype, and why is text usually `object`?
4. Why is `df["title"].str.len()` better than a `for` loop over rows?
Explain these back to me in chat before you build.
 
## Step 2: load the posts into a DataFrame (cell 6)
 
Same streaming loop as session 1 (files → lines → `json.loads`), but keep **only these 7 fields** from each post: `id`, `created_utc`, `subreddit`, `title`, `selftext`, `author`, `removed_by_category`.
 
1. For each post, build a small dict with just those keys and add it to a list. Use `.get` for `removed_by_category`, because it can be missing.
2. After the loop, make the DataFrame from the list of dicts in one call.
**Predict first** (write it in a comment): `df.shape`. What dtype will `created_utc` be? What about `title`?
 
**Check:**
- `len(df) == 171_756`
- `df.dtypes`, `df.head()`
- `df.memory_usage(deep=True).sum()` in MB
## Step 3: convert timestamps (cell 7)
 
Turn `created_utc` (seconds since 1970) into a real datetime column. The function is `pd.to_datetime`; look up its `unit` and `utc` parameters.
 
**Predict:** the new dtype.
**Check:** min and max match session 1 (2024-11-01 and 2025-12-30).
 
## Step 4: removed-category table (cell 8)
 
For each subreddit, show what share of posts falls in each `removed_by_category` value, with not-removed (`NaN`/`None`) as its own row.
 
Hint: `groupby` + `value_counts`. Look at the `normalize` and `dropna` parameters.
 
**Check:** removed shares land in the 43–69% range from session 1.
 
## Step 5: read samples (cell 9 + reading time, ~40 min)
 
Take ~20 random posts **per category**, including not-removed as a control group. Show `subreddit`, `title` and the first 200 characters of `selftext`.
 
- Hint: `groupby(...)` then `.sample(...)`. Set `random_state=42` so you get the same sample next time.
- Edge case: what happens if a category has fewer than 20 posts? Find out before it crashes.
- Tip: `pd.set_option("display.max_colwidth", 200)` so the text isn't cut off.
While reading, ask of each category:
- Are these real on-topic posts, spam, or duplicates?
- **`moderator` hypothesis:** are many of these removed because they belong in a megathread? If so, they still show what people were talking about, which is what we want.
## Step 6: decision (cell 10, markdown)
 
For each category, write one line: **keep / drop + why** (e.g. "automod_filtered: mostly real questions from new accounts, keep").
 
Optional: count how many posts survive a *strict* rule (keep only not-removed) and a *lenient* rule (yours). Write both numbers down.
 
## Step 7: build the `text` column (cell 11)
 
`text` = title + a newline + selftext, but:
- `"[removed]"` / `"[deleted]"` selftext counts as empty;
- link posts (`selftext == ""`) give just the title.
Hints: `.replace` or `.where` on the selftext column, then string `+` between whole columns. No loop.
 
**Predict:** what `text` looks like for an r/technology link post.
**Check:** print 3 random rows: one link post, one self post, one with `[removed]` selftext.
 
## Wrap-up (5 min)
 
- [ ] README → "Cleaning decisions": the removed-category rule and the `text` rule
- [ ] 3 lines in `LOG.md` (learned / still confused / next)
- [ ] Restart Kernel and Run All Cells, all green
- [ ] `git add` + `git commit`
**Next session (3):** text as data, tokenization and bag-of-words; drop very short posts, duplicates and bots; save the cleaned data to `data/processed/`.
 
---
 
## Notebook habits
 
- One idea per cell, with a markdown heading above each step.
- All imports go in cell 1 (add `import pandas as pd` there today).
- Now and then, run Kernel > Restart Kernel and Run All Cells. If everything still works, the notebook is reproducible.
- `[*]` means running or waiting, `[n]` is the run order, ■ interrupts, ↻ restarts (and forgets every variable).
- Don't print inside a loop over all posts.
- Before running a cell, write down what you expect to see. If you get stuck, paste the cell and the error.
## Starting Jupyter
 
From the project folder: `source .venv/bin/activate`, then `jupyter lab`. Check that the kernel in the top right is the `.venv` one.
 