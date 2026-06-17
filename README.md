# Teamwork Time Logger

A single-file web app for logging and reviewing hours on Teamwork via the API — without visiting the Teamwork site.

Deployed at: https://faisalalvi.com/teamwork-timelogger/

## Files

- **`index.html`** — v1 (stable baseline). Team View button and the Ticket Lookup "All team members" toggle are removed here.
- **`index-v2.html`** — active version. Everything in v1 plus ticket estimates and the unified caching engine described below. New work goes here; `index.html` is kept as a backup.

## Features

### Logging
- Week-at-a-glance view of all 7 days, entries grouped by project
- Days shown newest-first (today at top, then yesterday, …) for the current week
- Quick log form with project / task pickers, quick-set time buttons, date picker, billable toggle
- Quick Tasks card — one-click pre-fill of project/task from saved groups
- **Linear Quick Log** — paste a Linear URL or ticket ID, auto-maps to a Teamwork task via prefix mapping
- Inline edit / delete / duplicate on any entry
- Duplicate-ticket warning when logging a ticket already logged that day
- Auto-scroll to log form when a quick task card is clicked

### Ticket Estimates (v2)
- Assign a t-shirt estimate to any ticket ID: **XS / S / M / L / XL / XXL**
  - XS `<4h` · S `4–9h` · M `10–15h` · L `16–25h` · XL `26–40h` · XXL `40h+`
- Estimate badge appears anywhere a ticket ID is detected — day entries, Quick Log preview, and the manual Description field
- Badge shows **team hours logged vs the estimate cap** (e.g. `12/15h`), combining entries from all members
  - Amber when over 80% of cap, red when over cap
- Even with no estimate set, the badge still shows total hours logged (`8h logged`)
- Click the badge → opens Ticket Lookup (all-team) for that ticket
- `↗ Linear` link beside every badge opens the ticket in Linear (workspace slug configurable in Settings)
- Estimates stored in `localStorage` (`tw_ticket_estimates`)

### Reports & Lookup
- **Month Report** — monthly or custom date range breakdown by project, copy to clipboard
- **Ticket Lookup** — paste any Linear URL or ticket ID to find all time entries logged against it:
  - Your own entries, or all team members (1M / 3M / 6M / All time range selector — v2 only)
  - Flat or By Person grouped view, person filter chips, WHO column with avatar
  - Recent-ticket chips for quick re-search
  - Entry 🔍 button on any logged entry opens lookup pre-filled and auto-searches
- **Team View** — the whole team's entries for any date range:
  - Week navigation arrows (← →) with auto-load on open
  - Flat (default) or By Person grouped toggle, person filter chips
  - Search across person, task, description, task list, project
  - Columns: Who · Description (clickable links) · Task List · Start · End · Time
  - Member list: Dharmesh, Sumit, Pete, Vikram, Sid, Pushpak, Mukesh, Faisal
  - Per-range "🗑" button clears just that range's cache and reloads

### Performance & caching (v2)
- **Unified entry store** — one in-memory + `localStorage` store of all-team entries, bucketed by day. Every view (week, last-week comparison, badges, Team View, Ticket Lookup, Month Report) is a client-side filter of this store.
  - Past days are immutable → cached forever; only **today** is refetched per session
  - Week switching within the cached window = **zero** network calls
  - Page reload = one fetch (today); past days load instantly from `localStorage`
  - A write (log/edit/delete) refetches only the affected day
  - Range fetches are serialized and 429-retried with backoff; quota overflow prunes oldest days
- **Clear Cache** button in the header (full reset) and per-range clear in Team View
- **Bulk Adjust** — set weekly target hours per project, dry-run preview, one-click publish
- Dark/light theme, configurable font size, progress bar settings
- 7 PM end-of-day reminder notification, midnight day-change banner
- All data in `localStorage` — credentials never leave your machine except to the Teamwork API

### Settings (v2 additions)
- **Ticket estimate lookback** — months back to count team hours per ticket (1 / 3 / 6 / 12 / All time; default 3)
- **Linear workspace slug** — used to build Linear ticket URLs (default `a8c`)

## Setup

Open the file directly in a browser (`file://`) or serve it:

```bash
cd /path/to/this/repo
python3 -m http.server 8788
# open http://localhost:8788/index-v2.html
```

On first run, enter your Teamwork site (e.g. `yourcompany.teamwork.com`) and API key. Stored in `localStorage` — origin-specific, so set up separately for `file://` and the deployed site.

## Notes

- Single file per version: all HTML, CSS, and JS inline. No build step, no dependencies.
- `localStorage` is origin-scoped — credentials saved on the deployed site won't carry over to local `file://` access (and vice versa).
- Week view filters entries by the logged-in user's `person-id`.
