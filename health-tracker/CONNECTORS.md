# Connectors

This plugin uses two kinds of tools.

## 1. Local files (bundled — no setup in the connector store)

The plugin ships its own local-file connector called **`health-files`** (defined in
`.mcp.json`). It runs the standard Model Context Protocol filesystem server on your
machine and gives the dashboard read/write access to **one folder**:

```
~/HealthTracker
```

That's where all your data lives, as plain Markdown files. Nothing leaves your
computer. See `README.md` for the one-time setup (it needs Node.js installed).

If your data folder is somewhere other than `~/HealthTracker`, edit the last line of
`.mcp.json` to point at it.

## 2. Strava (you connect your own)

| Category        | Placeholder   | What to connect                          |
| --------------- | ------------- | ---------------------------------------- |
| Fitness / workouts | `~~fitness` | **Strava** (any Strava MCP connector)    |

Strava is read **live by Claude in chat** to fill in your workout calories and
VO₂ minutes when you ask about burn or net calories. Connect Strava from Cowork's
connector store. The visual dashboard does **not** talk to Strava directly (workout
data shows up when you ask Claude in chat), so the dashboard works fine even before
Strava is connected.
