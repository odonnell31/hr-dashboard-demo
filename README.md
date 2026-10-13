# HR Recruiting Dashboard — Claude Code Workshop

Build a recruiting dashboard from `HR_collected_data.xlsx` using Claude Code in your browser. No installs.

## Before you start (one time, ~3 min)

1. A claude.ai account on a **Pro, Max, or Team** plan (Claude Code in the browser isn't on the free plan).
2. A GitHub account.
3. **Fork this repo** (button at the top right of this page) so Claude can save work to your copy.
4. Go to **[claude.ai/code](https://claude.ai/code)** → sign in → **Connect GitHub**. When it offers to install the Claude GitHub App, install it on your account (don't skip).

## Start a session

1. At [claude.ai/code](https://claude.ai/code), pick **your fork** of `hr-dashboard-demo` in the repository selector.
2. Set the mode dropdown to **Auto** (or **Accept edits** if Auto isn't offered).
3. Paste **Prompt 1** below and press Enter.

---

## Prompt 1 — Dashboard + upload

```
Build a single-file web app, index.html, for an HR recruiting dashboard. No backend and no build step: plain HTML, CSS and JavaScript in one file. Load the libraries with exactly these two script tags, and use no other external resources:
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/plotly.js/1.58.4/plotly.min.js"></script>
Do not change these versions. cdnjs can't be reached from the Claude Code container, so other versions can't be checked there.

First, inspect HR_collected_data.xlsx in this repo (install openpyxl if you need it) so you understand the format. Each sheet is ONE open role. Rows 1-6 are key/value pairs in columns A/B: Team:, Open Role:, Date Posted:, Today's Date:, LinkedIn Applications:, Last Weeks Applications:. Row 7 is blank. Row 8 is a header row (Interview Date, Round, Candidate, Offer Presented, Offer Accepted) and the rows below it are the interview log. Round values are: phone screening, first round, second round, final round. Offer Presented is Yes, No, or blank.

Requirements:
1. An upload area (file input plus drag-and-drop) that accepts an .xlsx file and parses every sheet in the browser with SheetJS. Nothing is sent to a server.
2. One clearly named function parseRoleSheet(rows) that turns a sheet's rows into an object: {team, jobTitle, datePosted, today, totalApplications, lastWeekApplications, interviews: [{date, round, candidate, offerPresented}]}. Handle dates that arrive as JS Date objects or as strings. Keep it short and readable; I'll show it to students.
3. Compute per role: Days Open = Today's Date minus Date Posted (use the dates in the file, not the system clock); Candidates Interviewed = number of unique candidate names; Total Applications = LinkedIn Applications; Offers Presented = count of rows where Offer Presented is "Yes".
4. Dashboard view: title "Open Roles Dashboard" and a table with columns Team, Job Title, Days Open, Candidates Interviewed, Total Applications, Offers Presented, sorted by Days Open descending. Each row is clickable.
5. Clicking a row switches to a detail view in the same page (show/hide sections, no separate files) showing the job title and a "Back to dashboard" button. Leave the rest of the detail view as a placeholder for now.
6. Clean, professional styling that works on a laptop screen.

When done, commit index.html, publish it as an artifact, and give me the link.
```

Open the artifact link, upload `HR_collected_data.xlsx`, and the table appears. Click a row, then Back.

## Prompt 2 — Detail view

```
Now build out the detail view in index.html for the selected role:

1. Header: four metric cards for Job Title, Days Open, Offers Presented, Salary Range. Salary range is not in the data, so add a SALARY_RANGES lookup object keyed by job title: "Jr Data Analyst": "$65,000 – $80,000", "Data Engineer": "$115,000 – $140,000", "Marketing Analyst": "$70,000 – $90,000", "BI Developer": "$95,000 – $120,000", "Machine Learning Engineer": "$140,000 – $175,000". Default to "$80,000 – $110,000" for any other title.
2. A Plotly funnel chart titled "Interview Funnel": the number of unique candidates who reached each round, in this order: phone screening, first round, second round, final round.
3. A Plotly line chart titled "Applications by Week": one point per week from Date Posted to Today's Date. Generate the weekly counts randomly, but they must sum exactly to Total Applications and the final week must equal Last Weeks Applications. Seed the random generator from the job title so the chart does not change every time the view re-renders.
4. Below the charts, a "Recruiter Notes" section: a textarea and a "Save Note" button. Keep notes in a JavaScript object keyed by job title so they persist while switching between roles, and show a "Note saved" confirmation. No storage beyond that; this is a demo.

Use only Plotly features available in plotly.js 1.58.4 (the version loaded in Prompt 1). Keep the "Back to dashboard" button at the top. Commit and republish the artifact.
```

Reload the artifact, re-upload the xlsx, click **Machine Learning Engineer** (longest open), walk the funnel, type a note.

## Prompt 3 — Optional, if time remains

```
On the dashboard, highlight any row with Days Open greater than 45 in light red, and add three summary cards above the table: Total Open Roles, Total Applications across all roles, and Total Offers Presented. Commit and republish.
```

## Troubleshooting

**Charts are blank or say "Charts could not load"**

Paste this into the session:

```
The Plotly charts don't load in the artifact. Load Plotly with exactly
<script src="https://cdnjs.cloudflare.com/ajax/libs/plotly.js/1.58.4/plotly.min.js"></script>
and show a clear error message in the chart area if Plotly is undefined. Commit and republish.
```

**The upload does nothing, or the page says "SheetJS did not load"**

Your network may block cdnjs.cloudflare.com (some school or corporate networks do). Try another network or a personal hotspot.

---

## Run it locally instead

Download `index.html` from your fork and double-click it. It opens in any browser; upload the xlsx the same way. You need an internet connection the first time, because the two libraries load from cdnjs.cloudflare.com.

## Privacy note

The xlsx is parsed inside your browser tab. The published page has no backend and never uploads your file anywhere.
