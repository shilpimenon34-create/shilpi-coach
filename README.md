# Shilpi's Coach — web app

A standalone, installable version of the training coach. It runs entirely in the browser: no server, no account, works offline once opened.

## Files
| File | Purpose |
|---|---|
| `index.html` | The whole app (planner, workouts, progress, cycle tracker, library, settings) |
| `manifest.webmanifest` | Lets phones install it to the home screen as "Coach" |
| `sw.js` | Offline support (caches the app and fonts) |
| `icons/` | App icons |

## Put it online (free, about 2 minutes)
**Option A: Netlify Drop (easiest)**
1. Unzip this folder.
2. Go to https://app.netlify.com/drop and drag the unzipped folder onto the page.
3. You get a link like `https://something.netlify.app`. Sign in to keep it permanently and rename it (e.g. `shilpi-coach.netlify.app`).

**Option B: GitHub Pages**
1. Create a repository and upload these files to it.
2. Settings → Pages → Deploy from branch → `main` / root.
3. The app is served at `https://<username>.github.io/<repo>/`.

The app must be served over **https** for installing and offline mode to work (both options above are https).

## Install on the phone
- **iPhone (Safari):** open the link → Share → **Add to Home Screen**.
- **Android (Chrome):** open the link → menu ⋮ → **Install app** (or Add to Home screen).

It then opens full-screen like a normal app, with its own icon.

## Data
- Everything is stored on the device in the browser's storage. Nothing is sent anywhere.
- **Settings → Backup → Export backup** downloads a `.json` file. Do this occasionally.
- To move to a new phone: export on the old one, open the app on the new one, **Import backup**.
- Deleting the browser's site data (or uninstalling the home-screen app on some phones) erases the data, so keep a backup.

## Updating
Replace `index.html` on the host with a new version. Open the app once while online and it picks up the update. If you change the app, also bump `CACHE = 'coach-v1'` in `sw.js` to `coach-v2` so phones drop the old cached copy.
