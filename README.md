# Harry's Revision Hub v2.0

Year 9 end-of-year assessment revision tracker. Self-adjusting schedule, PWA, offline-capable.

## What's in here

- `index.html` — main app
- `manifest.json` — PWA configuration
- `sw.js` — service worker (offline support)
- `icon-192.svg`, `icon-512.svg` — app icons

## Features

- Countdown to 29 June 2026 exams
- Auto-detects Phase 1 (ramp-in, 1hr/night) vs Phase 2 (build, 1.5hr/night)
- 9 subjects with full topic checklists and curated resource links
- **Self-adjusting schedule**: missed slots auto-reschedule to current week
- Streak tracking
- Installable on phone home screen
- Works offline after first load
- All data stored in browser localStorage (private, no server)

## How the rescheduler works

1. Runs once per day on first app open (or via "Force reschedule" link in footer)
2. Finds slots from past days that weren't marked done
3. Sorts by priority (CS, Science, Geography first; Music last)
4. Tries to place each missed slot in:
   - Today first (if there's capacity)
   - Then upcoming weekdays this week
   - Then this weekend
   - Then next weekend as overflow
5. Will allow up to 30 min overflow on a day's capacity if needed
6. Slots that can't be placed anywhere are dropped (logged in notice)

Carried-over slots show "CARRIED OVER" badge. Overflow slots show "EXTRA" badge.

## Deploying to GitHub Pages

1. Create a new public GitHub repo (suggested name: `harry-revision-hub` or similar)
2. Upload all 5 files to the root of the repo:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.svg`
   - `icon-512.svg`
3. Go to Settings → Pages
4. Source: Deploy from a branch
5. Branch: `main`, folder: `/ (root)`
6. Save
7. Wait 1-2 minutes — GitHub will provide a URL like `https://yourusername.github.io/harry-revision-hub/`

Once live:
- Open the URL on Harry's phone
- Tap browser menu → "Add to Home Screen" (or you'll see the install banner inside the app)
- The app icon appears on his home screen and runs as a standalone app

## Local testing before deploy

You can't test the service worker by opening `index.html` directly (file://). For local testing:

```bash
cd harry-revision-pwa
python3 -m http.server 8000
```

Then open http://localhost:8000

## Troubleshooting

**"Service worker registration failed"** — only works over HTTPS or localhost. GitHub Pages serves HTTPS automatically. Won't work if you double-click the file.

**Rescheduler not running** — it only runs once per day. Use the "Force reschedule" link in the footer to trigger manually.

**Progress lost** — data is in browser localStorage. If you clear browser data, it's gone. If you uninstall the PWA on the phone, the data is preserved (it's stored against the domain, not the install).

**Updating the app after deploy** — push new files to GitHub; the service worker cache version is `harry-revision-v1.0`. Bump this in `sw.js` when shipping updates so users get the new version on next launch.

## Version

v2.0 — built 18 May 2026
