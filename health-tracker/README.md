# Health Tracker

A personal daily **health tracker** for Cowork — log your food, exercise, and
digestive (gut) symptoms by simply chatting with Claude, and see it all in a visual
dashboard. Everything is stored as **plain Markdown files in a folder on your own
computer**. Nothing is uploaded anywhere.

It does three things:

1. **Food + calories.** Tell Claude what you ate ("turkey sandwich and chips for
   lunch") and it logs it with estimated calories and macros, learning your common
   foods over time. The dashboard shows intake, burn, net calories, and a 7-day chart.
2. **Exercise + Strava.** Log manual workouts (weights, yoga) by chat or in the
   dashboard. Connect **Strava** and Claude pulls your runs/rides into the burn and
   net-calorie math when you ask.
3. **Digestion tracking.** Log gut episodes (diarrhea, loose stool, constipation,
   gas/bloating) with severity and Bristol score, then ask Claude — or use the
   dashboard's **"Find food correlations"** button — to see which foods tend to precede
   your symptoms.

---

## What you need

- **Cowork** (the Claude desktop app).
- **Node.js** installed on your Mac/PC. The dashboard saves through a small local
  file helper that runs on Node. If you don't have it: download the "LTS" installer
  from https://nodejs.org and run it once. (You don't need to know anything about Node —
  the installer is all it takes.)
- **Strava** *(optional)* — connect it later from Cowork's connector store if you want
  your runs/rides counted.

---

## One-time setup (about 3 minutes)

### 1. Make your data folder

Create a folder named **`HealthTracker`** in your home folder:

- **Mac:** `/Users/<you>/HealthTracker`  (i.e. `~/HealthTracker`)
- **Windows:** `C:\Users\<you>\HealthTracker`

This is where all your data lives, as plain `.md` files. You can open it any time.

> The plugin is preset to use `~/HealthTracker`. If you want it somewhere else, open the
> plugin's `.mcp.json` and change the last line (the folder path) to your folder, then
> restart Cowork.
>
> **Windows note:** if `npx` doesn't launch, change the `.mcp.json` `command` from
> `"npx"` to `"cmd"` and make the first argument `"/c"` followed by `"npx"` (the standard
> Windows form for npx-based MCP servers).

### 2. Install this plugin

Open the `health-tracker.plugin` file in Cowork and click **Install**. Then **restart
Cowork** so the local file helper starts.

### 3. Connect the folder to Cowork

In Cowork, connect/select your `HealthTracker` folder for the session. This lets Claude
read and write your log files when you chat.

### 4. Open your dashboard

Just say to Claude:

> **"Set up my Health Tracker dashboard."**

Claude will create the **Health Tracker** dashboard (a live page you can pin and reopen).
It finds your data folder automatically.

### 5. Set your numbers

In the dashboard, open **⚙ Calorie-burn settings** and enter your weight, height, and age.
These drive the resting-calorie (BMR) math. *(The formula uses the male equation; if you
need the female version, just tell Claude and it'll adjust your net-calorie reports in chat.)*

### 6. (Optional) Connect Strava

From Cowork's connector store, connect **Strava**. After that, ask Claude things like
"what's my net today?" and it will fold in your Strava workouts. The dashboard itself
works fine with or without Strava.

---

## How to use it

**By chatting (works anywhere, even on mobile):**

- "I had two eggs and toast at 7:30 and a turkey sandwich for lunch."
- "Log 45 minutes of weight training, about 280 calories."
- "Log a bout of diarrhea this morning, Bristol 6, felt like it was the dairy."
- "What have I eaten so far today? How many calories left?"
- "What's my net calorie balance today?" *(pulls in Strava if connected)*
- "What foods might be triggering my stomach?"

**In the dashboard:**

- Add food (leave macros blank to let Claude estimate them), exercise, and gut episodes.
- See net calories, macros, VO₂ minutes, and your 7-day chart.
- Click **🔎 Find food correlations** to scan which foods precede your diarrhea/loose
  episodes. Treat results as *leads to watch, not proof* — correlation isn't causation,
  and it needs a couple of weeks of consistent logging to mean anything.

Logging by chat and logging in the dashboard write the **same files**, so they always
stay in sync.

---

## Where your data lives

Inside your `HealthTracker` folder:

```
HealthTracker/
├── food-log/
│   ├── 2026-06-13.md        ← one file per day (food + manual exercise)
│   └── _library.md          ← your personal food database (auto-built)
└── symptom-log.md           ← rolling log of gut episodes
```

These are normal Markdown files — open, back up, or sync them however you like.

---

## A couple of notes

- This is a personal tracker, **not medical advice**. The food↔symptom correlation is a
  rough statistical hint to discuss with a doctor or dietitian, not a diagnosis.
- The dashboard reads/writes only your one `HealthTracker` folder. Nothing leaves your
  computer.
- If the dashboard says it "couldn't find your Health Tracker folder," make sure Node is
  installed, the folder exists at `~/HealthTracker` (or your edited path), and you
  restarted Cowork after installing.
