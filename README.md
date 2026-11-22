# M32 Service Setup

A dependency-free static web app for planning an M32 service setup. It can be hosted on GitHub Pages or opened locally in a modern browser.

## Run locally

Open `index.html` in a modern browser. No build step, package installation or server is required.

## Current workspace capabilities

- **Reusable source library:** define performers and instruments independently of a particular service; choose mono or stereo pair and identify Lead Vocal / Backing Vocal roles.
- **Service lineup:** select library items for the current service and assign their physical input source/number independently from their M32 desk channel. Stereo assignments reserve adjacent input numbers and desk channels.
- **Stage layout:** add stageboxes with configurable socket counts, edit socket labels, associate sockets with library sources and drag equipment around a top-down canvas.
- **Patch panels:** add panels with configurable port counts and labels; connect stagebox sockets to panel ports, then panel ports to Local, AES50-A, AES50-B or Card physical M32 inputs. Direct, normalled, half-normalled and thru connection types can be recorded.
- **Signal-path trace:** service assignments show the configured route from stagebox socket through the patch panel to the M32 input and desk channel when the matching route is present.
- **Full configuration backup:** export/import one versioned JSON configuration containing the library, service lineup, stage layout, stageboxes, patch panels, socket/port labels, physical connections and metadata. Import is validated and requires confirmation before replacing the current workspace.
- **Browser persistence:** changes are saved in local browser storage. Export a backup to transfer the setup to another browser or device.
- **M32 scene reference:** optionally load a scene file locally to inspect channel, bus, DCA and output-route names. The scene is not uploaded.

## Configuration and privacy

The complete configuration format is `m32-service-configuration`, schema version 1. The app stores it locally in the browser and exports it as JSON. Importing a file replaces the current workspace only after basic format/structure validation and user confirmation.

Scene files and configuration data are not sent to a server. Do not commit real scene files, venue-specific labels, performer details, credentials or other private material to this public repository.

## Known limitations / next increments

- This is a planning workspace, not an M32 remote-control application. It does not connect to the console or send OSC.
- Patch paths are explicit planning records. The app does not discover physical wiring, determine whether a panel is actually normalled, or electrically validate a connection.
- Collision checks cover duplicate physical-input and desk-channel allocations in the service lineup and duplicate patch-panel/M32 destinations. The operator must still confirm real-world wiring and console configuration.
- The stage canvas currently positions equipment visually; it is not a scale drawing or cable-length planner.
- Native M32 `.scn` generation is **not implemented**. A known-good base scene and verified M32 syntax will be required before that is attempted. Preserve the original scene and test any future generated file on the actual console before live use.
- Future work should deepen import validation, support richer front/rear patch-panel views, provide a more complete end-to-end patch audit (including stereo pairs), and add automated browser tests.

## Publish

Publishing remains manual through GitHub Actions:

1. Open the repository's **Actions** tab.
2. Select **Publish M32 Setup**.
3. Choose **publish to dev** or **publish to prod**, then run the workflow.

- **publish to dev** publishes the current build under `/dev/` and leaves the production site and version history untouched.
- **publish to prod** publishes the current build at the site root and also creates a retained version under `/versions/`.
- Production publication is deliberately separate from development work. Do not publish without explicit approval.

All GitHub Actions must use Node.js 24-compatible actions; do not introduce actions that depend on deprecated Node.js 20.
