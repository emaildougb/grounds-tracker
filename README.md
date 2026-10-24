# NCEMS Facilities and Grounds

One phone app for buildings and grounds at Stations 51, 51.5, 52 and 50. Two sections, switched at the top:

- **Facilities** (Nick Stafford and the BCs): report a problem, work orders, the scheduled service calendar, monthly walkthroughs, budget, replacement planning.
- **Grounds** (Tyler Komm): the Grounds Tracker list, same as before.

Everything saves to the Facilities and Grounds SharePoint site, so the data is NCEMS's, backed up by Microsoft, and visible in Teams.

**Version 2.17**

## Links

- Live: [https://fng.northcountryems.org/](https://fng.northcountryems.org/) (GitHub address: [https://emaildougb.github.io/grounds-tracker/](https://emaildougb.github.io/grounds-tracker/))
- Upload: [https://github.com/emaildougb/grounds-tracker/upload/main](https://github.com/emaildougb/grounds-tracker/upload/main)
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

**Phone vs desktop.** On a phone, Admin keeps the input screens only: Overview, Work, Schedule, Grounds and Buildings (fill in install year, life, cost). A blue bar says "Use desktop for full admin", with a **Show here** button for when you really need it. On a computer or in Teams (the app checks the device, not the window size) Admin also has **Dashboard** and **Budget**.

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

Upload the files to the repo (or push), GitHub Pages redeploys in about a minute. The app updates itself (v2.13 and later). Phones still on an older version need one last delete and re-add of the home screen icon.

## Changelog

### v2.17
- Haptics: a light tick when you tap a button, link, tab or card, the same way the EP Checker does it. iPhone needs iOS 18 or newer with System Haptics on (Settings, Sounds & Haptics). Android phones vibrate. Nothing else changed

### v2.16
- New phone icon: NCEMS green and olive split corner to corner with a white building and tree, NCEMS across the bottom (matches the Open Shifts icon). `apple-touch-icon.png` is the new icon and `icon-512.png` is the big one Android uses for the icon and the launch screen. Delete the old icon from the home screen and add it again to see it

### v2.15
- Kim Reid added to the admin (manager) list. Admins are now the chiefs (Doug, Derek, Dennis, Bryce), Chuck Andrus, Kim Reid, and Nick Stafford

### v2.14
- Sign in once: when opened from the hub or CG Staff with `?login_hint=`, it tries a quiet sign in, then one hop through Microsoft with no typing if you're already signed in there. Remembers who signed in, so the account picker is skipped next time

### v2.13
- The app checks for a newer version when it opens and whenever it comes back to the front (at most once a minute). If nothing is being typed it reloads itself; if a form, Fill In, a walkthrough or the budget plan is open, it shows "New version is ready" with an Update button instead.

### v2.12
- New station: Station 51 Lower Bays (pole barn, metal roof). NCEMS pays all upkeep (100%); the Town of Yacolt uses the bays by contract (80/20) and is responsible for problems it causes. FD13 has no share.
- When an admin opens the app, it adds any missing station to the Station choice column on the five facility lists (SharePoint REST, needs edit rights).
- Update the workbook reads the station columns from the Budget tab and needs the five-station Facilities Log.xlsx.

### v2.11
- Report a problem: picking Other asks "What kind of problem?" and Send waits for it. The answer is saved at the top of Details as "Type: ...".
- On a computer, tapping Admin opens the Dashboard. Phones still open the Overview.

### v2.10
- Update the workbook also reads Kim's QuickBooks "6270 budget vs actual" export when it is saved in Teams General (any .xlsx with 6270 and "actual" in the name; newest wins). Read only. Copies her budget and year to date actual for 330, 410, 420, 450 (our 455), 470 and 480 into the "so far, from Kim's books" block on the Budget tab, with the sub account breakdown in Notes (480 shows the buildings and grounds part, 481 + 483). Capital rows are left alone.

### v2.9
- Budget > "Update the workbook" (admins). Copies building items (Facility Assets), active services (Facility Services) and the budget plan for the workbook's budget year into Facilities Log.xlsx in Teams General, and stamps the Chief Summary. Planning estimates, qty and "required by" typed in the workbook are kept by matching rows on app ID (A12, S7), or station + name the first time. Items removed in the app drop out. Work orders still reach the Change Log through the Power Automate flow.

### v2.8
- 2025 and 2026 reference numbers now come from Kim's 2026 Final Budget Line Items. 2025 line 480 corrected to $141,425 (the earlier $121,575 came from the scanned board packet).
- Capital line renamed to match Kim's books: 620 Capital building outlay (Fund 6273, 594 22 60 620). 2026 = $81,000 (Facilities Maintenance, lower bay door openers, grading, electrical). Old rows saved as 594620 read as 620.

### v2.7
- Budget plan shows the agency's whole budget line for the two prior years under each line (2025 from the Final Budget Breakdown, 2026 typed in from Kim) for reference. Managers update them with "Update the reference numbers".
- Removed Submit to the Chief and Approve. The request goes to Bryce and Kim through the Facilities Log workbook.

### v2.6
- Budget plan and budget overview now use Kim's account codes instead of building system categories: Fund 6270 lines 330 Building supplies, 410 Professional services, 420 Communications, 455 Rental and lease, 470 Public utility service, 480 Repair and maintenance, and Capital Fund 6273 line 594620 Capital: building and equipment.
- Work orders count against 480; work type "Project or upgrade" counts against 594620.
- Facility Budget list has a new BudgetLine column.
- Chuck Andrus and Nick Stafford are admins. Nick still opens on his Facilities To Do list.
- App lives at fng.northcountryems.org (github.io forwards there).

### v2.5
- Top buttons: **Facilities**, **Grounds**, and **Admin** (BCs and Chief only). Facilities To Do has repairs, services and walkthroughs; Grounds To Do has grounds tasks plus an All tasks tab. Tyler starts on Grounds, everyone else on Facilities, then it remembers.
- Back arrow goes to the last screen; it only goes to the NCEMS hub when there is nowhere left to go back to. Phone back gesture works the same way.
- Playwright test: 68 checks

### v2.4
- Admin > **Fill In** (desktop): step by step screen to enter last done, vendor and cost for every service, and year installed, life, replacement cost and condition for every building item. Saves each box as you go. N/A turns a service off or removes an item. Progress bars per step.
- Info > Check facilities setup: each list opens the right screen
- Overview shows a Getting started box that points to Fill In (on a phone it says to use a computer)
- Playwright test: 60 checks

### v2.3
- Phone vs desktop now detected by device, so a narrow window on a PC still gets full admin
- Info (i) panel says Phone version or Desktop version
- "Check facilities setup" shows "Everything is set up" with a Back button instead of a setup screen
- Wording says "this computer" on a PC instead of "this phone"
- Sign-in inside a Teams tab uses a popup, with an Open in browser link as a fallback

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
