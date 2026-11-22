# Service configuration workspace

Status: agreed design; implementation in progress.

## Purpose

Extend M32 Service Setup from a scene-inspection prototype into a reusable service configuration workspace. The workspace models the physical stage, the reusable performer/instrument library, the service lineup, and the mapping from physical inputs to M32 channel strips. A complete configuration must be portable independently of an exported M32 scene.

## Core terminology

- **Physical input**: a socket/input source on the M32 or connected stagebox. Examples include a local M32 input or an AES50 stagebox input. Model the source explicitly (for example, Local, AES50-A, AES50-B) and its input number.
- **Desk channel**: the M32 channel strip/fader (CH 01–32) receiving the physical input.
- **Stage box**: a named physical input device represented on the stage layout. It has a model/type, a display name, a stage position and a set of individually named input sockets.
- **Library item**: a reusable performer/instrument/source definition. It exists independently of whether it is selected for a particular service.
- **Service assignment**: the use of a library item in one service, with its physical input and desk-channel allocation.

## Physical signal path and patch panel

The configuration must model the complete physical signal path, not only the stagebox-to-console assignment:

**Performer / instrument → stagebox input → stagebox output → patch-panel port → patch-panel connection → M32 physical input → M32 desk channel.**

- Add one or more configurable patch panels to the venue/stage infrastructure model.
- Each patch panel has a name, optional make/model, a configurable number of ports and optional notes.
- Each port has a stable identity and a user-editable label/number.
- Allow a stagebox output to be connected to a patch-panel port, and a patch-panel port to be connected onward to an M32 physical input.
- Represent direct connections and, where required by the real installation, normalled/half-normalled or thru relationships. Do not assume a normalled connection unless the user configures it.
- Make every connection explicit and inspectable; show source, intermediate ports and destination.
- Detect multiple sources assigned to the same port/input, incomplete paths, incompatible connection types where known, and disconnected endpoints. Do not silently resolve conflicts.
- Support patch-panel layouts with front/rear or input/output views where useful, while retaining a single underlying port/connection model.
- Include patch panels, port definitions, connection relationships and labels in the full configuration import/export.
- The stage layout should visually show stageboxes and patch panels in their physical positions. A separate patching view should make the signal route and port-to-port connections easy to inspect and edit.
- Keep physical signal routing separate from the M32 desk-channel assignment: the final M32 input is then assigned to a channel strip/fader.

## Functional requirements

### 1. Reusable source library

Each library item supports:
- Stable unique ID and editable display name.
- Source category / instrument / performer role.
- Optional performer name and notes.
- Mono or stereo configuration.
- For vocals: Lead Vocal or Backing Vocal classification. Non-vocal sources do not need a vocal classification.
- Default physical input source and input number (or a pair for stereo).
- Default desk channel (or an appropriate pair for stereo).

The library may contain more items than the 32 desk channels. Only items assigned to the current service consume desk channels.

### 2. Service lineup and channel allocation

- Select any subset of library items for a service.
- Assign physical inputs independently from desk channels.
- Support mono and stereo sources.
- A stereo source occupies two adjacent physical input sockets and two adjacent desk channels unless a verified M32 capability allows a different arrangement.
- Detect and clearly report duplicate physical input use, duplicate desk-channel use, out-of-range values and invalid stereo allocations.
- Preserve unused library items and their settings when the service lineup changes.
- Make it clear which inputs and desk channels are unused.

### 3. Visual stage layout

- Provide a top-down stage canvas.
- Allow users to add, rename, move and resize stage boxes.
- A stage box represents a physical device, not merely a decorative area.
- Configure each stage box with a name, device/type, number of input sockets and optional notes.
- Each socket can be given a meaningful channel/source name and associated with a library item or service assignment.
- Place stage boxes at user-defined positions on the canvas.
- Display assigned source/channel names on or beside the relevant stage box.
- Allow non-stagebox elements or labels (for example, drum position, keys, vocal positions, FOH) as a later extension; do not conflate these with physical input hardware.
- Stage layout coordinates and box definitions are saved in the complete configuration package.

### 4. Complete configuration import/export

Provide a versioned JSON package that includes:
- Configuration format identifier and schema version.
- Reusable source library, including inactive/unselected items.
- Stage layout dimensions and all stage-box definitions, socket names and positions.
- Service lineup and assignments.
- Physical input source/number mappings and desk-channel mappings.
- Mono/stereo and vocal classifications.
- Relevant application preferences and metadata.

Import must validate the format and version before replacing current data. Invalid files must not partially overwrite the current workspace. Provide a preview/confirmation step before applying an imported configuration.

Export must download the complete package as one file. The existing service-profile JSON export may remain as a separate diagnostic/interchange export, but must not be presented as a full backup.

### 5. Native M32 scene generation (separate increment)

- Generate a genuine M32 text .scn file from a known-good user-supplied base scene.
- Preserve unrelated scene settings, including buses, DCAs, effects and output routing unless explicitly changed by the user.
- Apply validated channel names, physical input patching, mono/stereo linking and any required channel parameters.
- Do not infer undocumented scene tokens. Verify syntax against authoritative M32 documentation and controlled fixtures.
- Keep the original uploaded scene unchanged.
- Clearly warn that generated scenes must be tested on the actual console before live use.
- Scene generation is separate from exporting/backing up the full service configuration.

## Storage and privacy

The app is a static browser application. Configuration data should be stored locally in the browser for convenience, with explicit full-package export/import for backup and transfer between devices. Do not add a server or upload user scene files or performer information. Do not commit real scene files, actual performer details or venue-specific private information to this public repository.

## Delivery increments

1. Establish the versioned configuration data model, full import/export and browser-local persistence.
2. Build the reusable source library and service lineup with independent physical-input and desk-channel assignment, mono/stereo support and validation.
3. Build the visual stage canvas with configurable stage boxes/sockets and patch panels, connected through an explicit physical signal-path model.
4. Build a patching view for tracing and editing stagebox-to-patch-panel-to-M32 connections.
5. Implement and test native .scn generation as a separately validated increment.

## Acceptance principles

- A library with more than 32 entries can be stored and exported even though no service can allocate more than the available desk channels.
- Changing a service lineup does not delete or overwrite unused library entries.
- Physical input and desk channel are visibly separate fields and can differ.
- Stereo allocations are validated for both physical inputs and desk channels.
- Stage-box socket labels are editable and remain associated with their stage-box identity.
- Patch panels and their ports can be configured, positioned and connected to stagebox outputs and M32 physical inputs.
- A user can trace a signal from its stage source through every patch-panel connection to the M32 physical input and desk channel.
- Conflicting, incomplete or disconnected patch paths are clearly reported.
- Export then import round-trips the entire configuration without losing library entries, stage boxes, positions, names or assignments.
- Import errors leave the current workspace unchanged.
- No private scene or performer data is transmitted to a server.
