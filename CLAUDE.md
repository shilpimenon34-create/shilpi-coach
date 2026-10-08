# Shilpi's Coach — project brief for Claude Code

Personal training web app for Shilpi. Her profile and health notes live in `CLAUDE.local.md` (git-ignored; the repo is public, so never commit personal details). Owner of the project: the Director (Raivent). Treat outputs as drafts he will refine.

## Stack
- Single-file app: `index.html` (HTML + CSS + vanilla JS, no build step, no framework).
- PWA: `manifest.webmanifest`, `sw.js` (offline cache), `icons/`.
- Storage: `localStorage` key `shilpi-coach.v1` holds `{state, logs}`; the in-progress workout is under `shilpi-coach.v1.active`. Backup = JSON export/import in Settings.
- The `Store.connect()` code path for `window.claude` is the claude.ai artifact version's sync; it is inert on a normal host. Keep it unless asked to remove it.
- Fonts: Google Fonts (Archivo display, Figtree body). Only external dependency.

## Hard rules (from the original brief)
1. Sessions must realistically fit 30–40 min. `estimate()` + `fitBudget()` enforce 38 min; never add exercises without re-checking the estimate.
2. Back-conscious: exercises with `back:2` are never auto-programmed; no jumping, no loaded spinal flexion.
3. Progression is double progression and is *recommended*, never forced: increase only after 2 clean sessions at the top of the range (or 1 at "Easy"), only when readiness is green, with a "Stay at X kg" option.
4. Pain: "sharp" removes strength work for the day and recommends professional assessment. Never write diagnostic language.
5. One genuine rest day per week; never used for catch-up. Missed sessions are re-planned, never stacked.
6. Same muscle group never trained hard on consecutive days (see `adj()` / `seqScore()`).
7. Cycle tracking is optional, never cancels training, and predictions are labelled estimates, not contraception.

## Code map (all in index.html `<script>`)
| Area | Functions / data |
|---|---|
| Exercise library | `E(...)` entries → `LIB`; patterns in `PATTERNS` |
| Demo animations | `ANIM` poses, `ANIM_OF` exercise→animation, `figInner()`, `fig()`, `figLoop()` |
| Session templates | `TYPES` (L1, U1, L2, F1 strength; PZ, YR, PC, MR, OPT timed; REST) |
| Weekly planner | `canonical()`, `planWeek()`, `seqScore()`, `isMissed()` |
| Readiness | `readinessFrom()`, `ctxFor()`, `sessionTypeFor()` |
| Session build | `buildSession()`, `chooseSlot()`, `painAlt()`, `targetFor()`, `fitBudget()`, `timedBlocks()` |
| Progression | `processLog()`, `stallContext()` |
| Cycle | `cycStats()`, `cycleInfo()`, `cycleDayClass()`, `predictedStarts()`, `periodStarted()` |
| Views | `viewToday`, `viewPlayer`, `viewTimedPlayer`, `viewWeek`, `viewProgress`, `viewCycle`, `viewLibrary`, `viewSettings`, sheets |
| Events | one delegated `click` handler on `data-act`; `input`/`change` handlers on `data-in` |

## Conventions
- Colors only via CSS tokens on `:root` (light) with dark overrides; keep both themes working.
- Every new form control gets a stable `id`.
- Keep the app working at 360 px width; no horizontal scroll.
- After editing `index.html`, bump `CACHE` in `sw.js` (`coach-v1` → `coach-v2` …) so installed phones update.
- Verify before finishing: `node --check` on the extracted script, then open the page locally (`npx serve .`) and click through Today → Check in → Start → log a set → Finish.

## Deploy
- **Netlify:** `npx netlify-cli deploy --prod --dir .` (first run: `npx netlify-cli login`, then create/link a site).
- **GitHub Pages:** every push to `main` deploys via `.github/workflows/pages.yml`, which publishes only the app files (`index.html`, `sw.js`, `manifest.webmanifest`, `icons/`). Add any new app file to that list.
- Must be served over https for install + offline.
