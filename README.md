# JSA app — Onsite filter & bulk worker checkout (prototype)

Clickable prototype of the People tab's new **Onsite** filter category and the bulk
check-out flow that follows it. Built on the Field Control Analytics design system.

## What's in here

| File | Purpose |
|---|---|
| `index.html` | Self-contained prototype. No build step, no network calls, no dependencies. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. |

## Deploy to GitHub Pages

```bash
git init
git add .
git commit -m "Add JSA onsite filter prototype"
git branch -M main
git remote add origin git@github.com:<org>/<repo>.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Build and deployment → Source: Deploy from a
branch**, branch `main`, folder `/ (root)`. The prototype is served at
`https://<org>.github.io/<repo>/`.

To keep it in a subfolder of an existing repo, copy `index.html` and `.nojekyll` into
e.g. `docs/` and set Pages to the `/docs` folder.

## Running locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

## The flow

1. Filter sheet opens on **Onsite** — every worker currently checked in, alphabetical,
   one checkbox each, **Select All** at the top. By Company and By Trade are unchanged.
2. Cap of **20** per check out: the line above the list shows the running count, the
   remaining checkboxes disable at 20, and Select All fills to 20 and stops.
3. **Apply** returns to the People tab showing only the selected group, with a
   "N of 20 selected" bar (Edit reopens the filter, Clear exits selection mode) and an
   X on each row to remove that worker.
4. **Check out N workers** asks for confirmation, then reports success. Checked-out
   workers drop off the Onsite roster.

**Restart flow** (above the phone) resets the prototype.

## Notes for engineering

- Worker data is hardcoded sample data in the page; there is no API layer.
- Existing flows, permissions and the By Company / By Trade filters are untouched.
- Open questions: whether a partial failure is possible on commit, and whether an undo
  window should follow the success screen.
