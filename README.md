# Teamwork Time Logger

A single-file local web app for logging hours on Teamwork via the API — without visiting the Teamwork site.

## Features

- Week-at-a-glance view of all 7 days with logged entries grouped by project
- Quick log form with project / task pickers, quick-set buttons, and billable toggle
- Inline edit / delete on any entry
- **Bulk Adjust** — set weekly target hours per project, dry-run preview with editable times, one-click publish to Teamwork
  - Extends existing entries proportionally (capped at 150% of original per entry)
  - Respects a 9-hour daily cap
  - Snaps additions to 15-minute increments
  - Manual override on any row in the dry run

## Running

```bash
cd /path/to/this/repo
python3 -m http.server 8788
# open http://localhost:8788/teamwork-timelogger.html
```

On first run, paste your Teamwork API key. It's stored in `localStorage` on your machine — never transmitted anywhere except the Teamwork API itself.

## Notes

- All times respect WordPress-style local timezone (`current_time` equivalent in JS).
- All API calls are scoped to the authenticated user via `userId` filter, so totals only show *your* hours, not the whole team's.
