# M32 Service Setup — first cut

A dependency-free static web app intended for GitHub Pages.

## Run locally
Open `index.html` in a modern browser, then select an M32 text scene file (`.scn`) from your computer.

## Publish
Publishing is manual through GitHub Actions:

1. Open the repository's **Actions** tab.
2. Select **Publish M32 Setup**.
3. Choose **publish to dev** or **publish to prod**, then run the workflow.

- **publish to dev** publishes the current build under `/dev/` and leaves the production site and version history untouched.
- **publish to prod** publishes the current build at the site root and also creates a retained version under `/versions/`. The version is generated as `vYYYY.MM.DD-RUN` using the UTC date and GitHub Actions run number.
- Previous version folders are retained across later deployments.

The repository's Pages source is **GitHub Actions**. The workflow builds a Pages artifact and deploys it using GitHub's Pages deployment actions. It also maintains the `gh-pages` branch as an archive of the published files so that version folders can be carried forward between deployments; that branch is not the Pages deployment source.

The public site URLs are:

- Production: `https://jaredchurch.github.io/m32-setup/`
- Development: `https://jaredchurch.github.io/m32-setup/dev/`
- Versioned production: `https://jaredchurch.github.io/m32-setup/versions/<version>/`

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
