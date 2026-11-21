# 01_eda plan (session 1)

Goal for today: look inside the raw files before changing anything. No cleaning and no pandas yet, just finding out what I downloaded. Each cell has a check so I know it worked.

## Cell 1: setup (done)

Imports (`json`, `datetime`, `timezone`, `Path`) and a sorted list of the three raw files, built from `Path("..") / "data" / "raw"` and `.glob("*.jsonl")`.

Check: runs without errors and lists 3 files.

## Cell 2: count posts and date range (done)

Loop over the three files and, for each one, go through every line counting posts and tracking the smallest and largest `created_utc`. Print one line per file with the subreddit, count, earliest date and latest date.

Result: r/OpenAI 57,180, r/artificial 19,818, r/technology 51,539. The dates run from 2024-11-01 to 2025-12-30. Dec 31 is missing because the download tool's end date is exclusive (written up in the README).

## Cell 3: field names (done)

Load only the first post of one file and print its keys.

1. `with open(files[0]) as fh:`, then `fh.readline()` to read just the first line
2. `first_post = json.loads(first_line)` (keep the whole dict, not only the keys)
3. print `len(first_post)` and `list(first_post.keys())`
4. extra: print `first_post["title"]` and `repr(first_post["selftext"])`

Check: the list includes `title`, `selftext`, `author`, `created_utc`, `subreddit` and `id`. There are dozens of other fields I won't need.

## Cell 4: odd values (done)

For each file, count the posts where:

- `selftext` is `""` (link posts)
- `selftext` is `"[removed]"` or `"[deleted]"`
- `author` is `"[deleted]"`

The structure is the same as cell 2: outer loop over files, inner loop over lines, counters reset for each file. Print each count as a percentage of the file's posts too.

Check: this shows how much real text each subreddit has, and whether removed posts explain why r/OpenAI has about 4 times more posts than I estimated from the New tab.

## Cell 5: findings (markdown cell) (done)

Press Esc, then M. Write 3 to 5 bullet points in plain English, for example "r/technology: X posts, Y% link posts, Z% removed." These are my first documented cleaning decisions.

## Wrap-up

- [ ] 3 lines in `LOG.md`: what I learned, what's still confusing, what's next
- [ ] `git add` and `git commit` (`data/` is git-ignored, and the README describes how to get the data)

## Notebook habits

- One idea per cell, with a markdown heading above each step.
- All imports go in cell 1.
- Now and then, run Kernel > Restart Kernel and Run All Cells. If everything still works, the notebook is reproducible.
- `[*]` means running or waiting, `[n]` is the run order, ■ interrupts, ↻ restarts (and forgets every variable).
- Don't print inside a loop over all posts.

## Starting Jupyter

From the project folder: `source .venv/bin/activate`, then `jupyter lab`. Check that the kernel in the top right is the `.venv` one.

Before running a cell, write down what I expect to see. If I get stuck, paste the cell and the error.
