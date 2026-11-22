# M32 Service Setup — first cut

A dependency-free static web app intended for GitHub Pages.

## Run locally
Open `index.html` in a modern browser, then select an M32 text scene file (`.scn`) from your computer.

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
