# SiteCheck RC — Concrete Site Inspection Companion

A installable, offline-capable PWA covering reinforcement reference/calculators and concrete
inspection tools, referenced to **AS 3600:2018**. Designed for one-tap access from an NFC tag.

## What's in this folder

```
index.html                 the whole app (HTML/CSS/JS, no build step, no external dependencies)
manifest.json               PWA manifest (name, icons, theme colour)
sw.js                        service worker — caches the app shell for offline use
icons/icon-192.png
icons/icon-512.png
icons/icon-512-maskable.png
```

Everything runs client-side. There's no server, database, or account — it just needs to be
**hosted somewhere reachable over HTTPS** (required for service workers / installability), and
opened once online so the service worker can cache it.

## 1. Host it (pick one — all free)

**GitHub Pages** (recommended, simplest to keep updating):
1. Create a new GitHub repo, e.g. `sitecheck-rc`.
2. Upload all files in this folder, keeping the `icons/` subfolder.
3. Repo Settings → Pages → Deploy from branch → `main` / root.
4. Your app will be live at `https://<username>.github.io/sitecheck-rc/`.

**Netlify / Vercel (drag-and-drop):**
1. Sign in, choose "deploy manually" / drag-and-drop deploy.
2. Drag this whole folder in.
3. You'll get a URL like `https://sitecheck-rc.netlify.app`.

**Your own server / company intranet:**
Just copy the folder to any static file host — it's plain HTML/JS, no build step required.

## 2. Install it as an app (once hosted)

- **Android (Chrome):** open the URL → menu → "Install app" / "Add to Home screen".
- **iPhone (Safari):** open the URL → Share → "Add to Home Screen".
- After installing once online, the app shell (tables, calculators, checklist) works **offline**
  — useful on sites with poor signal. Checklist entries are stored on-device (localStorage) and
  aren't shared between devices.

## 3. Write it to an NFC tag

Use any NFC-writing app (e.g. **NFC Tools** on Android/iPhone):
1. Open NFC Tools → Write → Add a record → **URL / URI**.
2. Enter your hosted URL, e.g. `https://<username>.github.io/sitecheck-rc/`.
3. Write to the tag, hold your phone to it once to confirm.

Tapping the tag will open the installed app directly (or the site in-browser if not yet
installed — from there the person can install it themselves).

**Sticker placement tip:** laminate/heat-shrink the tag and mount it somewhere it'll survive a
site environment — inside a site office door, on a tablet/hard-hat case, or on a laminated card
in the site folder.

## Updating the content later

Edit `index.html` (all the reference data lives in the `BARS`, `SQUARE_MESH`, `RECT_MESH`,
`COVER_TABLE`, `COVER_ENV`, `DEFECTS` and `CHECKLIST_GROUPS` arrays near the top of the
`<script>` block) and re-upload. Bump `CACHE_NAME` in `sw.js` (e.g. `sitecheck-rc-v2`) so
installed devices pick up the change next time they're online.

## Important — verify before relying on this in the field

This tool is a **field reference and QA aid**, not a design tool or a substitute for the project
structural drawings and specification:

- Bar/mesh areas are per AS/NZS 4671 (D500N) and the reinforcement reference table you supplied.
- Cover values are simplified from AS 3600:2018 Section 4 / Table 4.10.3.2 for common cases —
  always confirm exact exposure classification and nominal cover against the project durability
  specification and structural drawings.
- Development length and lap length calculators implement the **basic** (unrefined) formulas of
  Cl 13.1 and Cl 13.2 for straight D500N bars only. They do **not** cover hooked/cogged bar
  reductions, the transverse-steel refinement (k4/k5), bundled bars, or epoxy-coated/lightweight
  concrete multipliers — for those cases, or any non-standard condition, refer to the project
  engineer.
- The current uploaded copy of AS 3600:2018 could not be read when this app was built (only the
  reinforcement area reference table was accessible), so clause values were sourced from general
  engineering knowledge and cross-checked against multiple independent references rather than
  transcribed directly from the standard. **Cross-check the cover table and clause formulas
  against your own copy of AS 3600:2018 (with current amendments) before relying on this for
  sign-off**, and treat this as a fast reference/estimate tool rather than a certified
  calculation record.
