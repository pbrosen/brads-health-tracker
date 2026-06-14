# Connectors

This plugin uses two kinds of tools.

## 1. Local files — the Filesystem extension (you install it once)

The dashboard saves to your computer through the official **Filesystem desktop extension**.
It's a one-click install from Claude's **Settings → Extensions** directory and is fully
self-contained — it bundles its own runtime, so there's **no Node, no terminal, and no
config files** to deal with.

During install, point it at **one folder** — your `~/HealthTracker` folder — and that's
where all your data lives, as plain Markdown files. Nothing leaves your computer. See
`README.md` (steps 1–2) for the click-through.

> **Tool-name note (for Claude at setup):** the dashboard talks to the Filesystem extension
> via `window.cowork.callMcpTool(...)`. The exact tool prefix the extension registers under
> can vary by machine/version. When the setup skill creates the dashboard, it sets the
> dashboard's `FS_PREFIX` (and the artifact's `mcp_tools`) to match the actual connected
> filesystem tools (e.g. `read_text_file` / `write_file` / `list_directory` /
> `read_multiple_files` / `create_directory` / `list_allowed_directories`). The default
> assumes the connector is named `filesystem`.

## 2. Strava (you connect your own)

| Category        | Placeholder   | What to connect                          |
| --------------- | ------------- | ---------------------------------------- |
| Fitness / workouts | `~~fitness` | **Strava** (any Strava MCP connector)    |

Strava is read **live by Claude in chat** to fill in your workout calories and VO₂ minutes
when you ask about burn or net calories. The visual dashboard counts Strava in its burn
number only if activity files exist in `HealthTracker/strava/activities/` — set up the
optional **Strava auto-sync** task (see `skills/health-log/references/strava-sync.md`) to
keep those fresh. The dashboard works fine even before Strava is connected.
