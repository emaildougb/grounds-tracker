# NCEMS Grounds Tracker

Phone app for the Facilities and Grounds "Grounds Tracker" SharePoint list. Tyler marks tasks Start, Done, or Not done, changes who it's assigned to, adds work notes and photos. Every change writes straight to the SharePoint list, so the "Grounds Tracker Alerts" flow and the log still fire.

**Version 1.2**

## Links

- Live: [https://emaildougb.github.io/grounds-tracker/](https://emaildougb.github.io/grounds-tracker/)
- Demo (sample data, no sign-in): [https://emaildougb.github.io/grounds-tracker/?demo=1](https://emaildougb.github.io/grounds-tracker/?demo=1)
- Repo: [https://github.com/emaildougb/grounds-tracker](https://github.com/emaildougb/grounds-tracker)
- Upload: [https://github.com/emaildougb/grounds-tracker/upload/main](https://github.com/emaildougb/grounds-tracker/upload/main)
- SharePoint list: [https://northcountryemsorg.sharepoint.com/sites/FacilitiesandGrounds](https://northcountryemsorg.sharepoint.com/sites/FacilitiesandGrounds)

## Files (upload ALL of these)

- `index.html` (the whole app)
- `manifest.json` (home screen app mode)
- `logo.png`, `apple-touch-icon.png` (icons)
- `README.md` (this file)

## One-time setup

### 1. Create the repo

1. Make a new public repo on GitHub named `grounds-tracker` under emaildougb.
2. Upload the files above.
3. Settings > Pages > Source: Deploy from a branch, `main`, `/ (root)`. Save.
4. Wait for it to go green. The app lives at https://emaildougb.github.io/grounds-tracker/

### 2. Register the app in Microsoft Entra (about 5 minutes)

Needs someone who can create app registrations in the NCEMS Microsoft 365 tenant (a Global Admin or Application Administrator).

1. Go to [https://entra.microsoft.com](https://entra.microsoft.com) and sign in with your NCEMS account.
2. Left menu: Identity > Applications > **App registrations** > **New registration**.
3. Fill it in:
   - Name: `NCEMS Grounds Tracker`
   - Supported account types: **Accounts in this organizational directory only (Single tenant)**
   - Redirect URI: pick platform **Single-page application (SPA)** and enter `https://emaildougb.github.io/grounds-tracker/` (trailing slash matters)
4. Click **Register**.
5. On the Overview page, copy the **Application (client) ID**. Also check the **Directory (tenant) ID** says `153dc089-095e-4a20-8d10-39321a0aca09` (that's what the app is set to).
6. Left menu: **API permissions** > **Add a permission** > **Microsoft Graph** > **Delegated permissions**. Search and check **Sites.ReadWrite.All**. Click **Add permissions**. (User.Read is already there. Leave it.)
7. Still in API permissions: **Add a permission** > **SharePoint** > **Delegated permissions** > check **AllSites.Write** > **Add permissions**. (Lets the people picker search all of NCEMS and add new people to the site.)
8. Click **Grant admin consent for North Country EMS** and confirm. The status column should turn green.
9. Do NOT create a client secret or certificate. A phone app can't keep a secret, and SPA sign-in doesn't need one.

That's it in Entra. Nothing else changes in SharePoint or Power Automate.

### 3. Put the Client ID in the app

Open `index.html`, find this line near the top of the script:

```
clientId:  '',   // <-- paste the Application (client) ID from Entra here
```

Paste the ID between the quotes, save, upload. (On iPhone: open the file in the GitHub repo, tap the pencil, edit, Commit changes.)

If you skip this, the app shows a Setup screen where you can paste the ID on one phone for testing. Every phone would need it, so put it in the file for real use.

### 4. Add one column to the Grounds Tracker list (for type-in assignees)

Assigned To is a Person column, so it only takes people in your Microsoft 365. For a vendor or another department, the app saves the typed name to a separate text column. Add it once:

1. Open the Grounds Tracker list in SharePoint.
2. Click **+ Add column** > **Single line of text**.
3. Name it exactly `AssignedToOther` (one word, no spaces). This sets the internal name the app looks for.
4. Save. Then open the column settings and rename the display name to `Assigned To (Other)`. The internal name stays the same.
5. Optional: add it to the list view so you can see it on desktop.

If the column isn't there, the app still works. The type-in option just says the column is missing.

### 5. Install on Tyler's phone

1. Open the live link in Safari (iPhone) or Chrome (Android).
2. iPhone: Share > Add to Home Screen. Android: menu > Install app / Add to Home screen.
3. Open it from the home screen and sign in with his NCEMS Microsoft account. He stays signed in after that.

## Who can use it

The app signs in as the person holding the phone. It can only do what that person can already do in SharePoint. Tyler, Dennis, Derek and Doug are members of the Facilities and Grounds team, so they can edit the list. Anyone else gets a "No permission" message.

## How it works

- **This Week**: tasks due in the next 7 days, anything overdue that is still Not Started or In Progress, and anything finished today. Grouped by Station 1, Station 1.5 Admin, Station 2. Everyone / Mine filter (Mine = you are Assigned To or Responsible for Follow-up).
- **All Tasks**: every task, search box, filter Open / Not done / Done / All.
- **Colors**: gray Not Started, yellow In Progress, green Done, red Not Completed.
- **Start** sets TaskStatus = In Progress.
- **Done** sets TaskStatus = Done and CompletedOn = today.
- **Not done?** asks for the reason (required, quick-pick buttons or type it), then sets TaskStatus = Not Completed and ReasonNotCompleted.
- Each of those shows an **Undo** button for 7 seconds in case of a fat-finger tap.
- **Type-in Assigned To**: in the Assigned To picker, type a name that's not in the list (like `Rain City Gutters` or `Clark County PW`) and tap Assign. It saves to AssignedToOther and clears the Person field. Picking a real person later clears the typed name. No assignment email goes to a typed name, since there's no address to send to.
- **Details** (tap the task): instructions, frequency, category, change Assigned To and Responsible for Follow-up, add work notes, add photos, set back to Not Started.
- **Work notes** are added to the top of the WorkNotes column with date, time and name, like `10/07 9:52 AM  Tyler Komm: Mowed front lawn`.
- **Photos**: shrunk to 1600px JPEG on the phone, then saved to the site's Documents library in `Grounds Tracker Photos/Task <ID>/`. A line goes into WorkNotes saying a photo was added, so it shows in the log. See note below on why.
- **Staying current**: the app pulls fresh data when it opens, every 60 seconds while it's open, when you come back to it, and when you tap the refresh button. Edits made in SharePoint or by anyone else show up on their own.
- Last data is kept on the phone, so it opens instantly and still shows the list if signal drops (saving needs signal).

## The flow and the log

The app doesn't touch the "Grounds Tracker Alerts" flow. It edits list items the same way a person in SharePoint would, so the flow sees every change: red alerts on Not Completed, assignment emails on Assigned To changes, and a row in "Grounds Tracker Log.xlsx" for each change. "Modified By" will show the person who used the app.

## Known limits

- **Photos aren't list attachments.** Microsoft Graph can't add attachments to SharePoint list items, so photos live in a Documents library folder per task instead. They show up in the app under each task and in the Teams Files tab.
- **People picker** shows people the SharePoint site already knows (anyone who has opened the site or been assigned before). If someone's missing, have them open the SharePoint site once.
- If WorkNotes is set to "Append changes to existing text" in SharePoint, tell Doug. The app assumes a normal multi-line column.
- iOS caches hard. After every new upload, delete the home screen icon and add it again.

## Settings in index.html (CONFIG block)

- `clientId`: from Entra
- `tenantId`: NCEMS tenant (`153dc089-095e-4a20-8d10-39321a0aca09`)
- `siteHost` / `sitePath`: the Facilities and Grounds SharePoint site
- `listId`: `d8b3c948-246c-4371-8b8a-f27f2b1e2cbd`
- `photoFolder`: `Grounds Tracker Photos`
- `otherField`: `AssignedToOther` (type-in assignee column)
- `hubUrl`: back button target (`https://hub.northcountryems.org/`)
- `refreshSeconds`: 60

If the app ever moves to another URL (like a custom domain), add that exact URL as another SPA redirect URI in Entra > App registrations > NCEMS Grounds Tracker > Authentication.

## Changelog

### v1.2
- People picker searches everyone in the NCEMS Microsoft 365 directory (type 2+ letters), not just people the SharePoint site already knows. Works for Assigned To and Responsible for Follow-up
- Picking someone new adds them to the site automatically, then assigns them
- Needs one extra Entra permission: SharePoint > Delegated > AllSites.Write, with admin consent (see setup)
- Playwright smoke test (demo mode): directory search, assign new person, no duplicates after adding

### v1.1
- Assigned To picker: type any name (vendor, other department) and assign it. Saves to the new `AssignedToOther` text column
- Cards and details show the typed name; search finds it
- App checks if the column exists and says so if it doesn't
- Playwright smoke test (demo mode): type-in assign, switch back to a real person, existing vendor task display all pass

### v1.0
- First build
- This Week view grouped by station, overdue on top, color by status, summary tiles
- Start / Done / Not done? buttons with Undo
- Change Assigned To and Responsible for Follow-up
- Work notes and photo upload
- All Tasks view with search and filters
- Microsoft sign-in (MSAL 3.30, SPA, no secrets) and Microsoft Graph
- Auto refresh every 60 seconds and on app open
- Demo mode at `?demo=1`
- Playwright smoke test (demo mode): Start, Done, Not done with required reason, Mine filter, reassign, add note, add photo, All Tasks view, Setup screen, sign-in redirect URL all pass
