# M32 Service Setup

A dependency-free static web app for planning an M32 service setup. It can be hosted on GitHub Pages or opened locally in a modern browser.

## Run locally

Open `index.html` in a modern browser. No build step, package installation or server is required.

## Current workspace capabilities

- **Reusable source library:** define performers and instruments independently of a particular service; choose mono or stereo pair and identify Lead Vocal / Backing Vocal roles.
- **Service lineup:** select library items for the current service and assign their physical M32 input independently from their desk channel/fader. Stereo assignments reserve adjacent input numbers and desk channels.
- **Stage layout:** add stageboxes with configurable XLR and RJ45 socket counts, edit socket labels, associate sockets with library sources and drag equipment around a top-down canvas.
- **Input mapping:** map each stagebox XLR input directly to an M32 physical input in the Local, AES50-A, AES50-B or Card bank. Radio microphones can be represented by a stagebox named "Radio Mics". Desk channel/fader assignment remains separate.
- **Full configuration backup:** export/import one versioned JSON configuration containing the library, service lineup, stage layout, stageboxes, socket labels, physical input mappings and metadata. Import is validated and requires confirmation before replacing the current workspace.
- **Browser persistence:** changes are saved in local browser storage. Export a backup to transfer the setup to another browser or device.
- **M32 scene reference:** optionally load a scene file locally to inspect channel, bus, DCA and output-route names. The scene is not uploaded.

## Configuration and privacy

The complete configuration format is `m32-service-configuration`, schema version 1. The app stores it locally in the browser and exports it as JSON. Importing a file replaces the current workspace only after basic format/structure validation and user confirmation.

Scene files and configuration data are not sent to a server. Do not commit real scene files, venue-specific labels, performer details, credentials or other private material to this public repository.

## Known limitations / next increments

- This is a planning workspace, not an M32 remote-control application. It does not connect to the console or send OSC.
- Input mappings are explicit planning records. The operator must confirm real-world wiring and console configuration.
- The stage canvas positions equipment visually; it is not a scale drawing or cable-length planner.
- Native M32 `.scn` generation is **not implemented**. A known-good base scene and verified M32 syntax will be required before that is attempted. Preserve the original scene and test any future generated file on the actual console before live use.
- M32 output routing and output mapping are recorded for a later increment in [issue #1](https://github.com/jaredchurch/m32-setup/issues/1).

## Publish

Publishing is automated through GitHub Actions:

- Commits to `UAT` publish to https://jaredchurch.github.io/m32-setup/uat/
- Commits to `main` publish Production at https://jaredchurch.github.io/m32-setup/, retain a version under `/versions/`, then merge `main` back into `UAT` and trigger a UAT publication.
- Manual workflow dispatch is also available from the matching branch.
- The previous `/dev/` address is retired.

All GitHub Actions must use supported Node.js 24-compatible action runtimes; do not introduce actions that depend on deprecated Node.js 20.
