# Bulk worker check out from an Onsite filter (JSA app)

Phase 1

## 1. Description / Summary

Two additions to the People tab of the JSA app.

* **Onsite filter category.** A third option in the People tab filter, after By Company and By Trade. Lists all workers currently checked in who have not checked out, the same way By Company lists companies: checkbox per worker, alphabetical, Select All at the top, Clear all and Apply in the footer.
* **Bulk check out.** After Apply the selected workers appear in the People tab as a group. The user can remove workers, then check the group out in one action. Cap of 20; cap and count shown on screen.

The check out reuses the existing JSA scan-out, applied to multiple workers. No existing flow, permission or filter option changes.

## 2. Problem Statement / Purpose

Workers are scanned out one at a time, and there is no way to list who is onsite and act on them. At end of shift the user repeats the same action per worker.

Time-saving feature requested by many projects. The 20 cap is a performance ceiling, not a policy limit. Above 20, the user repeats the flow.

## 3. Requirements

### 3.1 Conditions

* All projects. No feature flag.
* Workers tab only; Visitors unchanged.
* Only workers eligible for scan-out appear in the Onsite list.
* Same users who can scan out today. No new permission.

### 3.2 UI / UX / Flow

#### A. Filter: Onsite Category

* **Placement.** Third in the rail.
* **Contents.** Checked in, not checked out, eligible for scan-out.
* **Order.** Alphabetical by first name, case-insensitive.
* **Row.** Checkbox, name, company on a second line. The company line distinguishes duplicate names.
* **Select All.** Checkbox at the top. Selects up to 20 and stops; tapping again clears.
* **Cap.** Line above the list: "Select up to 20 workers · N selected." At 20 it reads "Limit reached, 20 of 20 selected" and the remaining checkboxes disable. Unchecking one re-enables them.
* **Search.** Serves only the open category.
* **Rail count.** Items selected in that category, as today.
* **Empty state.** "No matching workers available."
* **Footer.** Clear all and Apply unchanged, except Apply is disabled while nothing is selected.

**Category exclusivity.** Onsite cannot be combined with the other categories. Tapping a blocked category does not switch the pane; a message appears: *"You cannot filter in this category when you already have Onsite workers selected."* Mirrored when a company or trade filter is selected and the user taps Onsite. Clearing the selection unblocks the rest.

#### B. People Tab: The Selected Group

* **List.** Only the selected workers. The Workers tab count switches to the group size.
* **Summary bar.** "N of 20 selected" over "Onsite · maximum 20 per check out", plus Edit (reopens the filter with the selection intact) and Clear (exits the group).
* **Row.** Standard People list content. No extra data.
* **Remove.** X removes the worker and unchecks them in the filter. Removing the last one exits the group.
* **Profile.** Chevron still opens the worker profile; the group survives the round trip.
* **Persistence.** Sticky across leaving the tab and backgrounding. Ends on Clear, on removing the last worker, or on a completed check out.
* **Action.** Sticky "Check out N workers" below the list, which opens the Select Zone step (3.2 C). The center scan button hides while it is shown.
* **Minimum.** One worker.

#### C. Zone Selection

Tapping "Check out N workers" opens the existing Select Zone step before anything is committed.

* **Sheet.** Title "Select Zone" with a close X. Same sheet used elsewhere in the app.
* **Select Zone.** Dropdown, empty by default, opens a searchable list of zones. Picking one shows a checkmark on the selected row.
* **Select Zone Code.** Dropdown, appears once a zone is selected, listing that zone's codes. Changing the zone clears the code.
* **Continue.** Disabled until both a zone and a zone code are selected.
* **Close.** X returns to the group with the selection intact and nothing checked out.
* **Header.** While the zone step is active the header replaces the project name with the zone context: "Select zone" until a zone is picked, then "Zone (Zone Code)", with "Last synced ..." beneath it. The project name returns once the check out completes.

#### D. The Check Out

* **Confirmation.** Follows Continue. States the count, the selected zone and zone code, and the time to be recorded. Back returns to the Select Zone step.
* **Commit.** Existing JSA scan-out per worker. What a scan-out writes does not change.
* **Success.** Reports the number checked out. Done returns to the unfiltered People list; those workers leave the Onsite list.
* **No undo.** Corrections go through the existing individual flow.

#### E. Partial Failure

Not all-or-nothing. Successes commit; failures are reported.

* The user returns to the selection screen with only the failed workers selected.
* Summary above the list: "17 of 20 checked out · 3 could not be checked out."
* Each remaining row carries its own error.
* The user can retry the group, remove workers, or handle them individually.
* A worker scanned out by someone else between Apply and commit fails as already checked out, not skipped silently.

## 4. Affected Areas

Only the JSA app People tab. The check out reuses the existing scan-out, so nothing downstream changes: same events, same code path, more than one worker in a row.

| Area | Impact |
| --- | --- |
| Billing / invoices | No changes |
| Main tabs UI | No changes |
| JSA app | All of section 3. People tab, Workers only. |
| Worker app | No changes |
| Worker portal, web and mobile | No changes |
| Project setup and information | No changes |
| Company setup and information | No changes |
| Worker onboarding and profile | No changes |
| Login flow | No changes |
| Account settings | No changes |
| Invitation flows | No changes |
| User / worker assignment flows | No changes |

## 5. Visibility / Access Levels

No permission changes. Whoever can scan a worker out today can use this; whoever cannot, cannot.

The Onsite list inherits the visibility of the People tab list it sits in. The user sees the onsite subset of the workers already shown to them, and can check out only those.

One exclusion on top of visibility: workers not eligible for scan-out never appear.

## 6. Labels / Statuses

None.

## 7. Notifications

None.

## 8. Scope

Bulk check-in will be done later.

## 9. Dependencies

None. Builds on the existing JSA scan-out and the existing People tab filter.
