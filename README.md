# M32 Service Setup — first cut

A dependency-free static web app intended for GitHub Pages.

## Run locally
Open `index.html` in a modern browser, then select an M32 text scene file (`.scn`) from your computer.

## Publish

Publishing is automated by GitHub Actions:

- A commit pushed to `UAT` publishes the build to the UAT site: https://jaredchurch.github.io/m32-setup/uat/
- A commit pushed to `main` publishes Production at the site root, creates a retained version under `/versions/`, and then merges `main` back into `UAT` before triggering a fresh UAT publication.
- Manual workflow dispatch remains available for UAT (from `UAT`) and Production (from `main`).

Production versions are generated as `vYYYY.MM.DD-RUN` using the UTC date and GitHub Actions run number. Previous version folders are retained across later deployments. The old `/dev/` address is retired.

The repository's Pages source is **GitHub Actions**. The workflow builds a Pages artifact and deploys it using GitHub's Pages deployment actions. It also maintains the `gh-pages` branch as an archive of the published files so that version folders can be carried forward between deployments; that branch is not the Pages deployment source.

The public site URLs are:

- Production: https://jaredchurch.github.io/m32-setup/
- UAT: https://jaredchurch.github.io/m32-setup/uat/
- Versioned production: https://jaredchurch.github.io/m32-setup/versions/<version>/

## Current capabilities
- Reads an M32 text scene locally in the browser; it does not upload the file.
- Parses input channel names, bus names, DCA names and output assignment lines.
- Provides an editable first-pass grouping of channels into stage areas. Initial groupings are name-based suggestions and must be checked against the actual stage.
- Exports a portable JSON service-planning profile.

## Deliberate limitations
- Planning prototype only: it does not connect to the M32, send OSC, change console state or generate a native `.scn` file.
- Stage assignments are provisional; the scene does not establish the physical position of every source.
- Output assignment tokens are shown as recorded and are not translated into physical destination labels.
- Do not load the exported JSON onto an M32. It is an application interchange format only.

## Privacy
Scene files are selected locally by the operator and are not uploaded by this application. Do not commit real scene files, venue-specific labels, credentials or other private material to this public repository. Use sanitised example files only, with explicit approval.
