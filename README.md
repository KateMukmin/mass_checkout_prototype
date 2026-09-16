# JSA app — Onsite filter & bulk worker check out

Clickable prototype and epic for the People tab's new **Onsite** filter category and the
bulk check-out flow that follows it. Built on the Field Control Analytics design system.

## What's in here

| File | Purpose |
|---|---|
| `index.html` | Self-contained prototype. No build step, no dependencies, no network calls. |
| `epic.html` | The epic (description, requirements, affected areas, access, scope). Print to PDF from the browser. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |

## Deploy to GitHub Pages

```bash
git init
git add .
git commit -m "Add JSA onsite filter prototype and epic"
git branch -M main
git remote add origin git@github.com:<org>/<repo>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Build and deployment → Source: Deploy from a
branch**, branch `main`, folder `/ (root)`. The prototype is served at
`https://<org>.github.io/<repo>/` and the epic at `.../epic.html`.

To keep it in a subfolder of an existing repo, copy the files into e.g. `docs/` and point
Pages at the `/docs` folder.

## Running locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

## The flow

1. Filter sheet opens on **Onsite** — workers currently checked in who have not checked
   out, alphabetical by first name, company on a second line, one checkbox each, and
   **Select All** at the top. By Company and By Trade are unchanged.
2. Cap of **20**: the line above the list shows the running count, the remaining
   checkboxes disable at 20, and Select All fills to 20 and stops. **Apply** is disabled
   until at least one worker is selected.
3. **Category exclusivity** — with Onsite workers selected, tapping By Company or By
   Trade shows a message instead of switching.
4. **Apply** returns to the People tab showing only that group: "N of 20 selected" bar
   with Edit and Clear, the tab count switched to the group size, an X on each row to
   remove a worker (which also unchecks them in the filter), and the chevron still
   opening the worker profile.
5. **Check out N workers** → confirmation → success, then back to the unfiltered list.
   Checked-out workers drop off the Onsite list. No undo.
6. **Partial failure** — toggle "Simulate partial failure" above the phone. Successes
   commit; the user returns to the selection screen with only the failed workers
   selected, a "17 of 20 checked out · 3 could not be checked out" summary, and an error
   on each failed row.

**Restart flow** (above the phone) resets the prototype.

## Notes for engineering

- Worker data is hardcoded sample data in the page; there is no API layer.
- The check out reuses the existing JSA scan-out; nothing downstream of a scan-out changes.
- Existing flows, permissions, and the By Company / By Trade options are untouched.
- Phase 2 is bulk check in; the filter and group UI should carry a second bulk action.
