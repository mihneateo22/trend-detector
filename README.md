# Reddit Trend Detector

## Question

Which AI/tech topics rose and fell in 2025, and do the spikes line up with real launches and news?

Events used to validate detected trends:
- DeepSeek R1 release (late January 2025)
- US "Liberation Day" tariffs (April 2, 2025)
- GPT-5 launch (August 7, 2025)

## Data

- **Source:** Arctic Shift download tool (arctic-shift.photon-reddit.com/download-tool), posts only (no comments), downloaded 2026-11-21.
- **Subreddits:** r/OpenAI (57,180 posts), r/artificial (19,818), r/technology (51,539). Total: 128,537.
- **Requested window:** 2024-11-01 → 2025-12-31.
- **Actual window:** 2024-11-01 00:03 UTC → 2025-12-30 23:57 UTC. The tool's end date is exclusive, so Dec 31, 2025 is missing.
- Nov–Dec 2024 is used only as warm-up history for the rolling baseline, not reported as trends.
- Raw files live in `data/raw/` (git-ignored, never edited).

## Cleaning decisions

1. **Incomplete weeks are dropped** at both ends before any weekly analysis. The first week (starting Fri 2024-11-01) has only 3 days, and the last (containing the missing Dec 31) is partial. Partial weeks would show artificially low counts and look like a false drop in activity. This also makes the missing Dec 31 irrelevant.
