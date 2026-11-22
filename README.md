# M32 Service Setup

A dependency-free static web app for planning an M32 service setup. It can be hosted on GitHub Pages or opened locally in a modern browser.

## Run locally

Open `index.html` in a modern browser. No build step, package installation or server is required.

## Current workspace capabilities

- **Reusable source library:** define performers and instruments independently of a particular service; choose mono or stereo pair and identify Lead Vocal / Backing Vocal roles.
- **Service lineup:** select library items for the current service and assign their physical M32 inputs. Stereo assignments reserve adjacent input numbers.
- **Stage layout:** add stageboxes with configurable XLR and RJ45 socket counts (editable after creation), edit socket labels, associate sockets with library sources, drag equipment around a top-down canvas and filter stageboxes by name.
- **Input mapping:** map each stagebox XLR input directly to an M32 physical input in the Local, AES50-A, AES50-B, Card or Aux 1–6 bank. Search/filter by stagebox name, input label, associated source name or physical input target; filter the list to show all, physically mapped or unmapped inputs. Radio microphones can be represented by a stagebox named "Radio Mics".
- **M32 channel mapping:** assign each of the 32 mono channel strips and six Aux channel strips to a physical input. The same physical input can feed multiple channel strips. FX Returns 1–4 Left/Right are listed as internal effects-engine returns and are not assignable to physical inputs.
- **Output mapping:** define destination equipment and systems (FOH, monitors, IEM, broadcast, recording or other) and map M32 Local XLR (1–16), AES50-A/B (1–48) and Card (1–32) physical outputs to them. Optional destination channel labels support Left/Right and named feeds. Duplicate physical outputs and repeated labels within a destination are validated.
- **Full configuration backup:** export/import one versioned JSON configuration containing the library, service lineup, stage layout, stageboxes, socket labels, physical input mappings, M32 channel-to-input mappings and metadata. Import is validated and requires confirmation before replacing the current workspace.
- **Browser persistence:** changes are saved in local browser storage. Export a backup to transfer the setup to another browser or device.
- **M32 scene import/export:** load a known-good `.scn` file locally to inspect channel, bus, DCA and output-route names, then export a new native `.scn` file using it as the template. The export applies CH 01–32 and Aux 1–6 physical input assignments while preserving all other scene entries. The scene is not uploaded.

## Configuration and privacy

The complete configuration format is `m32-service-configuration`, schema version 1. The app stores it locally in the browser and exports it as JSON. Importing a file replaces the current workspace only after basic format/structure validation and user confirmation.

Scene files and configuration data are not sent to a server. Do not commit real scene files, venue-specific labels, performer details, credentials or other private material to this public repository.

## Recent implementation notes

- Stagebox name search and input-mapping text search are available in the UAT application. The input-mapping view also provides a physical-input status filter (all, mapped or unmapped) These filters are display-only and do not alter configuration or routing records.
- Input mapping includes the M32 Aux 1–6 input bank in addition to Local, AES50-A, AES50-B and Card.
- Stagebox XLR and RJ45 port counts can be edited after creation. The default is three of each.
- The Pages publishing workflow gives each run attempt a unique artifact name, avoiding duplicate-artifact failures when a failed deployment is retried.

## Known limitations / next increments

- This is a planning workspace, not an M32 remote-control application. It does not connect to the console or send OSC.
- Input mappings are explicit planning records. The operator must confirm real-world wiring and console configuration.
- The stage canvas positions equipment visually; it is not a scale drawing or cable-length planner.
- Native M32 `.scn` export is implemented using a known-good, user-supplied scene as the template. Export changes only the input-source value in `/ch/NN/config` and `/auxin/NN/config` entries for mapped strips. It does not create a scene from scratch, alter FX returns, or configure internal source-to-physical-output routing. The original scene is never changed; test every exported file in M32-Edit and on the actual console before live use.
- Output mapping records physical output-to-destination assignments. It does not yet configure the M32 internal source-to-output routing matrix, send audio to the console, or generate native `.scn` output-routing commands.

## Publish

Publishing is automated through GitHub Actions:

- Commits to `UAT` publish to https://jaredchurch.github.io/m32-setup/uat/
- Commits to `main` publish Production at https://jaredchurch.github.io/m32-setup/, retain a version under `/versions/`, then merge `main` back into `UAT` and trigger a UAT publication.
- Manual workflow dispatch is also available from the matching branch.
- The previous `/dev/` address is retired.

All GitHub Actions must use supported Node.js 24-compatible action runtimes; do not introduce actions that depend on deprecated Node.js 20.
