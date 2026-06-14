# Strava auto-sync (optional)

This is an **optional** add-on. The dashboard's "Burned today" number counts Strava
workouts only if Strava activity files exist in `BASE/strava/activities/`. This task keeps
those files fresh so the dashboard's burn/net is accurate every time it's viewed. (Without
it, the dashboard shows resting + manual exercise only, and Strava is reflected when you
ask Claude in chat.)

## How the user enables it

The user says something like **"set up my Strava auto-sync"** (or "sync my Strava every
day"). When they do, Claude creates a scheduled task using the **task prompt** below.
Recommend a **daily** cadence (cron `0 6 * * *` = 6am) — plenty for calorie tracking and
light-weight. Hourly (`0 * * * *`) is possible but heavier and only runs while Cowork is open.

Prerequisites: the user's **Strava connector** must be connected and enabled, and their
Health Tracker folder must be set up (so `BASE` exists).

## Task prompt (Claude runs this on each scheduled run)

> Sync recent Strava activities into the Health Tracker so the dashboard's burn number is
> accurate. Steps:
>
> 1. **Resolve BASE** — the user's Health Tracker folder (default `~/HealthTracker`). The
>    Strava activity folder is `BASE/strava/activities/`. Create it if missing.
> 2. **Pick the window** — the last 7 days (today back 6 days).
> 3. **Read the user's HR zones once** if the Strava connector exposes them (e.g.
>    `get_athlete_zones`), so you can compute time at high heart rate. If zones aren't
>    available, set the VO₂/anaerobic minutes to `0:00` (calories still count).
> 4. **List activities** in the window from the Strava connector (e.g. `list_activities`).
>    For each activity, collect: sport/type, start date (`YYYY-MM-DD`), Strava activity id,
>    moving/elapsed duration, calories (kcal), and average & max heart rate. If the connector
>    provides HR streams or zone buckets (e.g. `get_activity_streams` / `get_activity_performance`),
>    compute minutes spent in the **VO₂ zone** and **anaerobic zone** (the top two zones).
> 5. **Write one file per activity** to
>    `BASE/strava/activities/YYYY-MM-DD-<sport-slug>-<name-slug>.md` using the EXACT format
>    in "File format" below. Slugs: lowercase, spaces→hyphens, strip punctuation, trim the
>    name to ~60 chars. If two activities collide on the same date+sport+name, append
>    `-<strava_id>` to the filename to disambiguate.
> 6. **Idempotency** — overwrite the file for an activity you've already written (don't
>    create duplicates). Skip activities with no usable data.
> 7. **Do not touch** `BASE/food-log/` or `BASE/symptom-log.md` — this task only writes
>    Strava activity files.
> 8. Finish with a one-line summary: how many activities were synced for the window.

## File format (must match exactly — the dashboard parses these fields)

The dashboard only needs these fields. Reproduce this structure precisely; the bolded
labels and the two HR-zone table rows are what the parser keys on:

```
---
strava_id: 18882432215
sport: "Run"
date: 2026-06-11
calories: 398
---

# Run — Hot run with E and T

**Date:** 2026-06-11  **Duration:** 40:00  **Calories:** 398

## Heart rate

- **Avg:** 134 bpm   **Max:** 162 bpm

**Time in zones**

| Zone | Time | % |
|---|---|---|
| **VO2 (155–165)** | **1:09** | 2.9% |
| Anaerobic (165+) | 0:00 | 0.0% |
```

Field rules:
- **Frontmatter** `date`, `sport`, `strava_id` are required (the dashboard reads these).
  `calories` is the kcal the dashboard counts toward burn.
- **`**Duration:**`** in `H:MM:SS` or `M:SS`.
- **`**Calories:**`** the activity's kcal (whole number).
- **`**Avg:**  N bpm`** average heart rate (used to estimate kcal if `calories` is ever missing).
- **VO₂ row** must be `| **VO2 (lo–hi)** | **M:SS** | pct% |` — both the label and the time
  bolded. **Anaerobic row** must be `| Anaerobic (hi+) | M:SS | pct% |` (not bolded). The
  dashboard adds these two times together for "VO₂max zone minutes." Use `0:00` for either
  if you don't have zone data.

Anything else in the file (distance, power, notes, extra zone rows) is fine — the dashboard
ignores it. Keeping only the fields above is enough.

## Notes

- This is the same local-files pattern the dashboard already reads on every view, so once
  the task has run, the burn number updates the next time the user looks at the dashboard.
- The exact Strava tool names depend on the user's connected Strava connector — discover
  and use whatever activity/stream/zone tools it exposes at runtime.
