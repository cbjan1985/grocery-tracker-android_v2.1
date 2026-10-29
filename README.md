# Grocery Tracker – Android project (Designed by CB)

This package builds the Android debug APK on GitHub (no Android Studio needed).
It wraps the web app in `www/index.html` (web version 1.7, unchanged).

## Upload to GitHub (website method)
1. On github.com create a **new repository** (private is fine). Do not add a README.
2. Extract this zip on your computer. Open the extracted folder: you should see
   `package.json`, `www`, `README.md` and a hidden folder `.github`.
3. In the new repo click **uploading an existing file** and drag in **everything inside** the
   extracted folder (not the folder itself). Commit to the `main` branch.
   - The folder `.github` is hidden. Windows: File Explorer > View > Show > Hidden items.
     Mac: press Cmd+Shift+. in Finder.
   - If `.github` still does not upload: in the repo click **Add file > Create new file**, type
     `.github/workflows/android-debug.yml` as the name, paste the content of
     `WORKFLOW_COPY/android-debug.yml`, and commit.
4. Repo **Settings > Actions > General > Workflow permissions**: choose
   **Read and write permissions** and Save (needed to publish the APK as a Release).

## Build the APK
1. Open the **Actions** tab. If asked, click to enable workflows.
2. Click **Build Android debug APK** > **Run workflow** > Run workflow.
3. Wait about 5-10 minutes for a green tick.
4. Get the APK (either way):
   - **Releases** (right side of the repo home page) > latest "Debug build" > `app-debug.apk`
   - or the run page > scroll to the bottom **Artifacts** > `grocery-tracker-debug-apk` (a zip containing the APK)
5. Copy the APK to your phone and install it (allow "install unknown apps" when asked).

If the run shows a red cross, open the failed step and send the last 30 lines.

## Git command method
```
git init && git add . && git commit -m "Grocery Tracker Android"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

## Progress and next steps
See `DEVELOPMENT_PROGRESS.md`. In a new session, ask Claude to read it and continue.
Signed release APK instructions will be added at the final milestone (M10).
