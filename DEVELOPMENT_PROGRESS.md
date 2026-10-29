# Grocery Tracker – Android (Phase 2) – Development Progress

Last updated: 2026-10-29 · Web app baseline: Grocery-Tracker-v1.7.html (kept unchanged in `www/index.html` until a milestone says otherwise)

## Approach (decided in M0)
- Wrap the existing single-file web app with **Capacitor 6** (same approach as the user's other Android app). The web code, calculations, data model and business rules stay as they are; native features are added through small adapter functions.
- **APK is built by GitHub Actions** (`.github/workflows/android-debug.yml`): it runs `npx cap add android`, `npx cap sync`, `./gradlew assembleDebug` and uploads `app-debug.apk`. The authoring sandbox has Node 22 and Java 21 but **no Android SDK, no Gradle and no internet**, so an APK cannot be built or tested inside it.
- **Data / backend:** the web app has no server. All data (`S.items`, `S.meds`, `S.stat`, `S.purchases`) lives in the browser store plus an optional JSON file. "Consistency between web and mobile" = both use the **same JSON schema and backup/restore + CSV/Excel import-export**; no live sync unless a backend is added later (not requested so far).

## Findings from inspecting the source (v1.7, ~154 KB, 500 lines)
| Area | Today (web) | What the Android app needs |
|---|---|---|
| Tabs | Inventory, Available, Purchase item, Expire items, Medicines, Stationery, KPI, Data & Templates | Keep all; mobile navigation redesign (M6) |
| Storage | `localStorage` + File System Access API (`showSaveFilePicker`) | Keep localStorage; replace file-picker save with native Filesystem/Share (M3) |
| Export / download | anchor `download` via `dl()` | Does not work reliably in Android WebView → Capacitor Filesystem + Share (M3) |
| Excel/CSV | SheetJS loaded from jsDelivr CDN | Bundle SheetJS inside the app so it works offline (M2) |
| Kannada auto-translation (non-catalog names) | `claude.use('sample')` – exists only inside Claude artifacts | **Needs replacement** (M4, decision pending) |
| Bill reading | `claude.use('sample')` with image | **Needs replacement** (M4, decision pending) |
| Expiry alerts | in-page popup | Keep + add Android local notifications (M5) |
| Logo | CB logo already embedded as base64 JPEG | Reuse for icon/splash (M7) |

## Milestones
| # | Milestone | Status |
|---|---|---|
| M0 | Inspect source, choose approach | ✅ Done |
| M1 | Capacitor project scaffold + GitHub Actions debug build | 🟡 Scaffold written; **CI build not yet run** (needs repo push) |
| M2 | Bundle SheetJS locally, remove CDN dependency (no logic change) | ⬜ |
| M3 | Native adapters: export/share files, import file picker, backup/restore, Android back button | ⬜ |
| M4 | Replace Claude-only features: Kannada translation + bill reading | ⬜ blocked on user decision |
| M5 | Expiry local notifications | ⬜ |
| M6 | Mobile UI: theme tokens, bottom navigation, card layouts, readable typography | ⬜ |
| M7 | Icon + splash ("Grocery Tracker", "Designed by CB", CB logo, watermark 10–20% opacity, no Kannada) | ⬜ |
| M8 | Decorative transparent images (groceries, fruit, vegetables, medicines, stationery) at 10–15% opacity | ⬜ |
| M9 | Test workflows: inventory, purchase list, Kannada, medicine stock, expiry, bill, import/export; fix bugs | ⬜ |
| M10 | Signed release APK instructions (keystore, signing config, build command) + final README | ⬜ |

## Pending decision (needed before M4)
How should the APK translate new item names into Kannada and read bills? Options: (A) user's own Anthropic API key entered once in the app (needs internet, small usage cost, best accuracy); (B) offline only: built-in catalog for Kannada, on-device OCR for bills (no cost, lower accuracy, more work).

## Known issues
- Package re-zipped flat (files at the zip root, README.md with upload steps, WORKFLOW_COPY/ for manual workflow creation) because the hidden `.github` folder is easy to miss when uploading.
- M1 CI: user ran the workflow but could not find the APK (cause not yet known: either the run failed, or the artifact was not located). Workflow updated (v2) to fail loudly if no APK is produced, upload build reports on failure and also publish the APK as a GitHub Release. Waiting for the user's run result.
- Web app items still pending from earlier rounds: Excel template to upload existing inventory; separate "existing stock" entry in the other add forms. They are web features and will be done in the web file first, then copied to `www/index.html`.
- No APK has been built or run yet.

## Test results
- M1: `www/index.html` is byte-identical to Grocery-Tracker-v1.7.html; `package.json` and `capacitor.config.json` parse as valid JSON; workflow YAML parses.

## Exact next steps
0. Replace `.github/workflows/android-debug.yml` with the updated version, re-run the workflow, then look at the Releases page (right side of the repo home page) or the run's Artifacts section. If the run is red, send the last lines of the failed step.
1. User pushes this folder to the Git repository (branch `main`) and runs the workflow *Build Android debug APK* (Actions tab → Run workflow). Download artifact `grocery-tracker-debug-apk`, install, and report whether the web app opens and works inside it. Mark M1 ✅ only after that.
2. Answer the M4 decision (A or B).
3. Next session: read this file, then do M2 (small, safe).

## Resume rule
New session → read this file → continue at the first milestone not marked ✅. Never restart or discard completed work.
