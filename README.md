# NCEMS Facilities and Grounds

One phone app for buildings and grounds at Stations 51, 51.5, 52 and 50. Two sections, switched at the top:

- **Facilities** (Nick Stafford and the BCs): report a problem, work orders, the scheduled service calendar, monthly walkthroughs, budget, replacement planning.
- **Grounds** (Tyler Komm): the Grounds Tracker list, same as before.

Everything saves to the Facilities and Grounds SharePoint site, so the data is NCEMS's, backed up by Microsoft, and visible in Teams.

**Version 2.2**

## Links

- Live: [https://emaildougb.github.io/grounds-tracker/](https://emaildougb.github.io/grounds-tracker/)
- Demo (sample data, no sign-in): [https://emaildougb.github.io/grounds-tracker/?demo=1](https://emaildougb.github.io/grounds-tracker/?demo=1)
  - See it as someone else: add `&as=nick`, `&as=tyler`, `&as=bryce`, `&as=dennis` or `&as=staff`
- Repo: [https://github.com/emaildougb/grounds-tracker](https://github.com/emaildougb/grounds-tracker)
- SharePoint site: [https://northcountryemsorg.sharepoint.com/sites/FacilitiesandGrounds](https://northcountryemsorg.sharepoint.com/sites/FacilitiesandGrounds)

The URL stayed the same so Tyler's home screen icon, the Microsoft sign-in setup and the Grounds alert flow all keep working.

## Files

- `index.html` (the whole app)
- `manifest.json` (home screen app mode)
- `logo.png`, `apple-touch-icon.png` (icons)
- `README.md` (this file)

## Crew and Admin

The app has two sides. The switch at the top only shows for BCs and the Chief.

- **Crew** (Tyler, Nick, crews): one **To Do** list per station, with a big circle to tap on each item. It mixes grounds tasks this week, repairs (Critical and Urgent first under "Fix it now"), building services coming due, and the monthly walkthrough. Tap the circle to finish it, tap the words to open details, notes and photos. Station chips at the top remember your choice. Big red **Report a problem** button. Second tab: **Walkthrough**.
- **Admin** (BCs and Bryce): **Overview** (facilities and grounds counts, what needs attention), **Work** (all work orders), **Schedule**, **Grounds** (every grounds task: this week, late or not done, open, done), **Budget**.

**Phone vs desktop.** On a phone, Admin keeps the input screens only: Overview, Work, Schedule, Grounds and Buildings (fill in install year, life, cost). A blue bar says "Use desktop for full admin", with a **Show here** button for when you really need it. On a computer or in Teams (900px wide or more) Admin also has **Dashboard** and **Budget**.

**Dashboard** (desktop): open work orders, services current, spent vs budget, grounds this week, building services status, open jobs by station, budget vs spent by station with the NCEMS/FD13 split, spending by month, replacements coming due, walkthroughs, and the critical and overdue lists. Hover any bar for numbers. **Print / PDF** prints just the dashboard. Direct link for a Teams tab: `https://emaildougb.github.io/grounds-tracker/?view=dash`

| Person | Sees | Extra |
|---|---|---|
| Doug, Dennis, Derek | Admin (can flip to Crew) | Builds next year's budget |
| Bryce | Admin (can flip to Crew) | **Approves** the budget |
| Nick Stafford | Crew | Can edit the schedule and building items |
| Tyler Komm and everyone else | Crew | |

Roles come from the email addresses in `CONFIG`. The app only does what the person's SharePoint permissions allow, so anyone who uses it needs to be a **member** of the Facilities and Grounds team.

## First time setup (once, by a BC)

No new Microsoft Entra steps. The app uses the same registration as the Grounds Tracker (Sites.ReadWrite.All and AllSites.Write, already consented).

1. Open the app and sign in as a BC.
2. You land on **Admin**. The app says Facilities isn't set up yet.
3. Tap **Set up Facilities**. It creates five SharePoint lists on the site and loads the starter data:
   - **Facility Work Orders**: repairs and jobs
   - **Facility Services**: 77 scheduled services across the four stations (fire alarm, extinguishers, HVAC, generator, backflow, bay doors...)
   - **Facility Assets**: 36 building items for replacement planning (roofs, HVAC units, water heaters, generators, bay doors...)
   - **Facility Checks**: monthly walkthrough results
   - **Facility Budget**: budget lines by year, station and category
4. Takes about a minute. Safe to run again: it only adds what's missing. If a column ever goes missing, Info > **Check facilities setup** puts it back.

Then fill in **Last done** dates on the Schedule tab and **install year, life and cost** on Budget > Buildings so due dates and the replacement forecast work.

## How Facilities works

### Home
Critical count, open jobs, overdue and due-soon services, and a big red **Report a problem** button. Lists what needs attention, what's coming due, and which buildings need their monthly walkthrough.

### Report a problem
Pick the station, say what's wrong, where, category, and **Critical / Urgent / Routine**. Optional photo. It becomes a work order assigned to Nick. Critical shows "call Dennis now."

### Work orders
Filters: Open, Critical, Mine, Waiting, Done, All. Grouped by station, Critical first.
- **Start**: In Progress
- **Waiting**: pick Parts / Vendor / Approval / Funding / Weather and add details
- **Done**: actual cost, date, vendor, what was done. The cost counts against that station's budget.
- Tap a card for priority, details, assignee (any NCEMS person or a typed vendor name), estimate, vendor, category, notes and photos. Cancel or reopen from there.
- Photos go to the site's Documents library under `Facilities Photos/WO <number>/`.

### Schedule
Every recurring service with its own due date, colored by state: red overdue, yellow due in 30 days, gray needs a last done date, green OK.
- **Done**: date, cost, who did it. Sets the next due date. If there was a cost it's logged as a finished work order so the budget counts it.
- **Work order**: creates a job for Nick (for things that need a vendor call). Finishing that work order updates the schedule too.
- Tap a service to edit frequency, vendor, cost, importance, or turn it off. **+ Add** for new ones.

### Checks
Monthly walkthrough per building, the same lines as the printed checklist. Each line is **OK** or **Problem**. A Problem needs a short note and is either **Fixed on the spot** or **Needs a work order** (optionally Critical). Answers save on the phone as you go, so a call in the middle doesn't lose anything. Submitting saves the walkthrough and creates the work orders. A building shows as due after 31 days.

### Budget
- **Overview**: pick a year. Budget, spent, committed (open job estimates) and left, by station with a bar, by agency using the cost split, by category, replacements coming due over 5 years, and recent spending.
- **Plan (next year)**: Dennis's October projection. Grid of category by station. **Fill suggested** puts in scheduled services for the year + replacements due that year + last year's repairs. **Save draft**, then **Submit to the Chief**. Bryce taps **Approve**, which locks it. Approving doesn't spend anything. Bryce can unlock it.
- **Buildings**: every building item with when it's due for replacement and what it'll cost. Tap to fill in install year, life, cost, condition, make, model, serial.

Cost split (NCEMS / FD13): Station 51 50/50, 51.5 75/25, 52 100/0, 50 0/100. Change `ncems` in `FSTATIONS` in the script if that changes.

## Grounds

Unchanged from Grounds Tracker v1.2, with stations now shown as Station 51, 51.5 and 52. The values stored in the list didn't change, so the alert flow and the Excel log keep working.

## Staying current and offline

Data refreshes when the app opens, every 60 seconds, when you come back to it, and on the refresh button. The last data is kept on the phone, so it opens instantly and still shows everything with no signal. Saving needs signal. If a save fails, the change is put back and a red message says why. SharePoint "busy" responses are retried automatically.

## Alerts

Grounds alerts run as before (Power Automate "Grounds Tracker Alerts"). Facilities alerts (Critical work order emails to Doug and Dennis, and a morning overdue digest) are a Power Automate flow on the Facility Work Orders and Facility Services lists. Set up after the lists exist.

## Settings (CONFIG block in index.html)

- `clientId`, `tenantId`: Microsoft sign-in
- `siteHost`, `sitePath`: the Facilities and Grounds site
- `listId`: Grounds Tracker list
- `photoFolder`, `facPhotoFolder`: photo folders
- `managers`, `budgetApprover`, `facilitiesWorker`, `groundsWorker`: role emails
- `maintTargetPct`: suggested yearly maintenance as a share of replacement value (3%, NRC range is 2 to 4%)
- `checkEveryDays`: walkthrough due after this many days (31)
- `refreshSeconds`: 60

## Deploying

Upload the files to the repo (or push), GitHub Pages redeploys in about a minute. iOS caches hard: delete the home screen icon and add it again after every update.

## Changelog

### v2.2
- Admin Dashboard (desktop and Teams), printable, `?view=dash` link
- Phone Admin trimmed to inputs: Overview, Work, Schedule, Grounds, Buildings, with "Use desktop for full admin" bar
- Playwright test: 50 checks

### v2.1
- Split into **Crew** (one To Do list per station with big check-offs, walkthrough, report button) and **Admin** (overview, work, schedule, grounds, budget)
- Only BCs and the Chief see Admin
- Admin Grounds tab: This week, Late or not done, Open, Done, All
- Playwright test (demo mode): 40 checks across Doug, Bryce, Nick, Tyler and staff, no page errors

### v2.0
- Facilities section: Home, Report a problem, Work orders, Schedule, Checks, Budget (overview, next year plan with Chief approval, building replacement planning)
- One-tap setup creates the five SharePoint lists and loads 77 services and 36 building items
- Roles by email: BCs, Chief approves budget, Nick (facilities), Tyler (grounds)
- Stations renamed 51, 51.5, 52, 50
- Automatic retry when SharePoint is busy
- Playwright test (demo mode): 33 checks across Doug, Bryce, Tyler and staff, no page errors

### v1.2
- People picker searches the whole NCEMS directory and adds new people to the site

### v1.1
- Type-in Assigned To (vendor or other department)

### v1.0
- First build of the Grounds Tracker app
