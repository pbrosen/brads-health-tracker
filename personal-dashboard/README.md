# Personal dashboard source

This folder is where I keep my **own** Food Tracker dashboard source under version control —
the Obsidian-backed one I actually use, separate from the friend-facing plugin.

To save it here, copy your live artifact in once:

```bash
cp ~/Documents/Claude/Artifacts/food-tracker/index.html ./index.html
git add personal-dashboard/index.html && git commit -m "snapshot personal dashboard"
```

Re-run that copy + commit whenever you change the dashboard, so git keeps every version.
