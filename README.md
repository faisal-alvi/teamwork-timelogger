# Teamwork Time Logger

A single-file web app for logging and reviewing hours on Teamwork via the API — without visiting the Teamwork site.

Deployed at: https://faisalalvi.com/teamwork-timelogger/

## Features

### Logging
- Week-at-a-glance view of all 7 days with logged entries grouped by project
- Quick log form with project / task pickers, quick-set time buttons, date picker, and billable toggle
- Quick Tasks card — one-click pre-fill of project/task from saved groups
- **Linear Quick Log** — paste a Linear URL or ticket ID, auto-maps to a Teamwork task via prefix mapping
- Inline edit / delete / duplicate on any entry
- Auto-scroll to log form when a quick task card is clicked

### Reports & Lookup
- **Month Report** — monthly or custom date range breakdown by project, with copy to clipboard
- **Ticket Lookup** — paste any Linear URL or ticket ID to find all time entries logged against it:
  - Searches your own entries (fast, no date limit) or all team members (with 1M / 3M / 6M / All time range selector)
  - Shows WHO column (with avatar) when searching all team members
  - Entry 🔍 button on any logged entry opens lookup pre-filled and auto-searches
- **Team View** — see the whole team's time entries for any date range:
  - Week navigation arrows (← →) with auto-load on open
  - Flat view (default) or By Person grouped view toggle
  - Person filter chips to isolate one team member
  - Search bar filters across person, task, description, task list, project
  - TeamWork-style columns: Who · Description (with clickable links) · Task List · Start · End · Time
  - Member list: Dharmesh, Sumit, Pete, Vikram, Sid, Pushpak, Mukesh, Faisal

### Performance & UX
- **5-minute API cache** — all GET responses cached in memory; auto-invalidated on any write
- **Bulk Adjust** — set weekly target hours per project, dry-run preview, one-click publish
- Dark/light theme, configurable font size, progress bar settings
- All data stored in `localStorage` — credentials never leave your machine except to the Teamwork API

## Setup

Open `index.html` directly in a browser (`file://`) or serve it:

```bash
cd /path/to/this/repo
python3 -m http.server 8788
# open http://localhost:8788
```

On first run, enter your Teamwork site (e.g. `yourcompany.teamwork.com`) and API key. Stored in `localStorage` — origin-specific, so set up separately for `file://` and the deployed site.

## Notes

- Single file: all HTML, CSS, and JS in `index.html`. No build step, no dependencies.
- `localStorage` is origin-scoped — credentials saved on the deployed site won't carry over to local `file://` access (and vice versa).
- Cache clears on any POST/PUT/DELETE so logged entries always appear fresh.
