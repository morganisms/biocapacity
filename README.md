# BioCapacity

A single-file web app for planning team capacity across a biotech / diagnostics project portfolio. It shows each person's weekly load as a heatmap so over-allocation is easy to spot before it becomes a schedule slip.

A companion to [BioPortfolio](https://github.com/morganisms/bioportfolio): BioPortfolio decides which projects matter most, and BioCapacity shows whether the team can staff them.

**Run it:** download `index.html` and open it in any modern browser. There's no install, no server and no external dependencies.

## Features

- **Capacity heatmap**: one row per person and one column per week, 12 weeks at a time. Each cell shows the share of that person's available time that is booked, colored by load. A "Whole team" row shows overall utilization, and the current week is outlined.
- **Summary**: flags who is over capacity in the next 4 weeks (with their peak load), the team's load this week, and who has room for more work.
- **Edit allocations**: select a name to see their projects and type a percentage into any week. Totals update as you type.
- **Bulk assign**: set the same percentage for a person and project across a range of weeks. Use 0 to clear.
- **People and projects**: add, rename and remove people and projects, set each person's availability (for part-time staff or standing commitments) and pick project colors.
- **Filtering and navigation**: page backward and forward by 4 weeks, jump back to this week, or show only people who are over capacity.
- **CSV import and export**: bring in allocations from Excel or Smartsheet with a preview before anything changes. See [CSV format](#csv-format).
- **Undo**: imports, bulk assigns and removals can be reversed with one click until you make your next edit.
- **Light and dark mode**: follows your system setting until you choose one.

## How load is calculated

Load = hours booked across all projects ÷ the person's availability.

A person with 80% availability who is booked 40% + 40% is at 100% load.

| Color | Load | Meaning |
|-------|------|---------|
| Slate blue | Under 70% | Room for more work |
| Green | 70–100% | Healthy |
| Amber | 101–120% | Stretched |
| Red | Over 120% | Over capacity |

Include a project for operations or routine work so it counts against capacity.

## CSV format

Export a CSV to get a ready-made template. Each row is one person, one project and one week:

| Column | Required | Notes |
|--------|----------|-------|
| Person | Yes | Matched to existing people by name (not case-sensitive) |
| Role | No | Updates the person's role when filled in |
| Availability % | No | 5–100. Updates the person's availability when filled in |
| Project | Yes | Matched to existing projects by name |
| Week of | Yes | `YYYY-MM-DD` or `M/D/YYYY`. Dates are moved to the Monday of that week |
| Percent of time | Yes | Whole numbers 0–200. `50` and `50%` both work |

Column order doesn't matter, and common alternatives such as Name, Week and Allocation are recognized. Comma, semicolon and tab separated files all work.

Importing shows a preview first, with line-numbered errors for any rows that can't be used. Choose one of two modes:

- **Merge** adds new people and projects and overwrites matching weeks. Everything else stays. A 0 clears that week.
- **Replace** makes the plan exactly what's in the file.

Values like `0.5` are rejected rather than guessed at, because they usually mean Excel formatted 50% as a decimal. Text that starts with `=`, `+`, `-` or `@` is prefixed with `'` on export so spreadsheets don't run it as a formula, and the prefix is removed on import.

## Data

Changes save automatically to your browser's local storage, so they stay on your machine and survive a reload. Clear browser data and they're gone, so export a CSV backup regularly. Exports are named with the date, for example `biocapacity-2026-10-03.csv`, and open in Excel with accented names intact.

The sample plan loads the first time you open the app. Use **Clear sample data** to start your own.

If the app is open in more than one tab, each tab picks up changes made in the others.

All saved and imported data is validated on load, and malformed records are dropped. If the saved plan can't be read at all, the app loads the sample plan instead and keeps the unreadable copy in browser storage under `biocapacity_v1_unreadable`, so nothing is overwritten.

## Limits

- 200 people and 100 projects per plan
- CSV imports up to 2 MB and 20,000 rows
- Names up to 80 characters
