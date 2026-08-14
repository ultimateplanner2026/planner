# Update — 2026-08-13

**build 2026-08-13 23:06** · regression 150/150 · sync 11/11 · reminders 15/15 · calendar 15/15

**Status: built and tested, NOT yet uploaded to GitHub.**

---

## New — Reminders

In **Edit**, on any task with a date, a **Reminder** row with two dropdowns:

| Dropdown | Choices |
|---|---|
| Type | No reminder · Alarm sound + alert · Silent alert only |
| Timing | At the time · 5 · 10 · 15 · 30 min · 1 hr · 2 hrs · 1 day before |

- **Mac, browser open** — alarm tone plus a banner you dismiss
- **iPhone, app open** — the same banner
- **iPhone, app backgrounded but alive** — a real iOS notification, and iOS
  applies your own sound and haptic settings

### What it deliberately does NOT claim

You asked for loud off-silent and vibrate on-silent. A web app cannot do that on
iPhone:

- `navigator.vibrate` does not exist in iOS Safari and never has
- No web API can read the silent switch
- iOS gives web apps **zero** background execution once the app is closed

Also: vibrate-on-silent is a **user setting** on iPhone (Settings → Sounds &
Haptics → Vibrate on Silent), not something derived automatically — so any menu
option promising it would be wrong about half the time. The options are labelled
honestly instead, and the dialog says so in plain text.

---

## New — Calendar (.ics) export

A **Calendar (.ics)** button beside Export. This is the answer to closed-app
alarms: iOS Calendar fires them itself, with correct loud/vibrate behaviour, for
free.

- Skips finished tasks and anything already past (iOS cannot schedule a past alarm)
- Carries your reminder timing across as a real calendar alarm
- Re-exporting **updates** your events instead of duplicating them
- On iPhone it opens the share sheet; on Mac it downloads a file you double-click

---

## Fixed — the week view could show seven days all dated `NaN`

`weekStartOf` had no defence against a missing `weekStart` setting. The result
was the string `NaN-NaN-NaN`, with **no error anywhere**. The week grid still
drew seven cells, so it looked completely normal while no task could ever appear
on any day.

A fresh install was never affected. The exposed path was a hand-edited
`planner.json` — and the docs tell you that file is safe to edit by hand.

---

## Fixed — a test that broke because five days passed

A check parked the view 20 days ahead and expected a task to still be in the
month view. From the 13th of a month, +20 days is the *next* month. Second time
this class has hit the suite, so it is now a written rule.

---

## To put this live

Upload `~/Planner/index.html` to
**https://github.com/ultimateplanner2026/planner** → Add file → Upload files →
Commit changes.
