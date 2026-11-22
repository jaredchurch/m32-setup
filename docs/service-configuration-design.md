# M32 Service Configuration Workspace

Status: current application design; implementation is in progress. The stagebox search, input-mapping search, Aux input-bank support, editable stagebox port counts and initial output-mapping increment have been implemented in UAT.

## Purpose

Provide a reusable planning workspace for an M32 service. The application models the source library, service lineup, physical stage layout, direct stagebox-to-M32 input mapping and separate M32 channel-strip-to-physical-input mapping. The complete configuration is portable independently of an exported M32 scene.

## Core terminology

- **Physical input:** an M32 input destination, identified by its bank and input number. Supported banks are Local, AES50-A, AES50-B, Card and Aux (inputs 1–6).
- **Stagebox:** a named physical input device represented on the stage layout. It has a display name, configurable XLR input sockets, optional RJ45 network sockets, a stage position and notes.
- **Stagebox input:** an individual XLR socket on a stagebox. Each socket can be labelled and associated with a source.
- **Input mapping:** the direct assignment from a stagebox XLR socket to an M32 physical input. There is no intermediate patch-panel equipment in the application model.
- **M32 channel strip:** a console fader/input channel that can be assigned to an M32 physical input. Supported assignable strips are CH 01–32 and Aux 1–6. FX Returns 1–4 Left/Right are internal effects-engine returns and are displayed but are not assignable to physical inputs in this mapping.
- **Library item:** a reusable performer/instrument/source definition that exists independently of whether it is selected for a particular service.
- **Service assignment:** the use of a library item in one service, with its physical input allocation.

Radio microphones are modelled as an ordinary stagebox named **Radio Mics**, with each receiver output represented by an XLR input socket. No separate radio-mic hardware type is required for the current design.

RJ45 sockets on stageboxes represent network/AES50 connections and are not treated as analogue audio input sockets in the input mapping matrix.

## Functional requirements

### 1. Reusable source library

Each library item supports:
- Stable unique ID and editable display name.
- Source category / instrument / performer role.
- Optional performer name and notes.
- Mono or stereo configuration.
- For vocals: Lead Vocal or Backing Vocal classification. Non-vocal sources do not need a vocal classification.
- Optional full M32 scene-channel configuration retained as a reference.
- Stereo pairs formed from two mono sources retain the left/right source configuration.

The library may contain more items than the console channel strips. Only items assigned to the current service consume physical inputs; channel-strip patching is configured separately.

### 2. Service lineup and physical input allocation

- Select any subset of library items for a service.
- Assign physical inputs to source items independently from the M32 channel-strip mapping.
- Support mono and stereo sources.
- A stereo source occupies two adjacent physical input sockets; its left/right physical inputs can be assigned to suitable channel strips.
- Detect and clearly report duplicate physical input use, out-of-range values and invalid stereo allocations.
- Preserve unused library items and their settings when the service lineup changes.
- Make it clear which physical inputs are unused.

### 3. Visual stage layout

- Provide a top-down stage canvas.
- Allow users to add, rename and move stageboxes.
- Configure each stagebox with a name, optional make/model, a configurable number of XLR input sockets and RJ45 network sockets, and optional notes.
- Stageboxes default to three XLR inputs and three RJ45 sockets; users can edit both port counts after creation.
- Each XLR socket can have a meaningful label and be associated with a library source or service assignment.
- Stereo source associations identify left and right sides.
- Place stageboxes at user-defined positions on the canvas. Provide a text filter to find stageboxes by name without changing their stored positions or configuration.
- Display assigned source names on or beside the relevant stagebox.
- Allow non-stagebox elements or labels (for example, drum position, keys, vocal positions or FOH) as a later extension; do not conflate these with physical input hardware.
- Stage layout coordinates and box definitions are saved in the complete configuration package.

### 4. Direct input mapping

- Provide a mapping view that lists every XLR input socket on every stagebox, with separate visible columns for the associated source-library name, stagebox name/socket label and M32 physical input target. For stereo library sources, show the Left/Right side. Text search must cover stagebox names, input labels, associated source names/categories and existing M32 physical input targets.
- Allow each stagebox XLR input to map directly to any available M32 physical input target in Local, AES50-A, AES50-B, Card or Aux (1–6).
- Provide a status filter for all inputs, inputs mapped to a physical M32 input, and inputs not mapped to a physical M32 input. This filter must not change stored mappings.
- Do not model a physical patch panel, intermediate port, normalled connection or thru path.
- Detect duplicate M32 physical input destinations and duplicate source socket mappings; clearly identify conflicts rather than silently resolving them.
- Keep stagebox-to-physical-input mapping separate from M32 channel-strip-to-physical-input mapping. The channel mapping is the authoritative fader assignment and allows one physical input to feed multiple channel strips.
- Include all stagebox socket labels and direct input mappings in configuration import/export.

### 5. M32 channel mapping

- Provide a dedicated mapping view for all 32 mono channel strips (CH 01–32), six Aux channel strips (Aux 1–6), and eight FX Return strips (FX Returns 1–4 Left/Right).
- Allow each CH and Aux strip to be assigned to any supported M32 physical input: Local, AES50-A, AES50-B, Card or Aux 1–6.
- Allow one physical input to feed multiple channel strips; each channel strip can have at most one physical input assignment.
- Display FX Returns as internal effects-engine returns. Do not offer physical-input assignments for FX Returns.
- Display the associated Source Library element name for each mapped physical input by following the M32 channel assignment through the stagebox-to-physical-input mapping to the stagebox socket's library association. Include stereo Left/Right identity and stagebox/socket context where available.
- Provide text search across channel names, mapped physical inputs, Source Library element names/categories, and associated stagebox/socket labels.
- Preserve channel mappings in full configuration import/export and browser-local persistence.

### 6. M32 output mapping

- Model M32 physical output sources and destinations separately from stagebox-to-M32 input mapping.
- Consider Local XLR outputs, AES50 outputs and Card outputs where applicable.
- Allow output destinations to be named and mapped to destination equipment or monitor systems.
- Keep output routing distinct from input mapping and desk-channel assignments.
- Validate duplicate/conflicting assignments and output ranges.

### 7. Complete configuration import/export

Provide a versioned JSON package that includes:
- Configuration format identifier and schema version.
- Reusable source library, including inactive/unselected items.
- Stage layout dimensions and all stagebox definitions, socket names and positions.
- Service lineup and assignments.
- Direct stagebox-to-M32 physical input mappings.
- M32 channel-strip-to-physical-input mappings, including CH 01–32 and Aux 1–6.
- Mono/stereo and vocal classifications.
- Relevant application preferences and metadata.

Import must validate the format and version before replacing current data. Invalid files must not partially overwrite the current workspace. Provide a preview/confirmation step before applying an imported configuration.

Export must download the complete package as one file. The existing service-profile JSON export may remain as a separate diagnostic/interchange export, but must not be presented as a full backup.

### 8. Native M32 scene export

- Generate a genuine M32 text .scn file from a known-good user-supplied base scene. (Implemented in the current increment.)
- Preserve unrelated scene settings, including buses, DCAs, effects and output routing unless explicitly changed by the user.
- Apply validated physical input assignments to CH 01–32 and Aux 1–6 by updating the input-source field in each `/ch/NN/config` or `/auxin/NN/config` entry. Preserve channel names, icons, colours and all other parameters. Mono/stereo linking and other channel parameters are not changed by this increment.
- Use the documented M32 input-source index ranges: Local 1–32, AES50-A 33–80, AES50-B 81–128, Card 129–160 and Aux 161–166. Require each mapped strip to have a matching config entry in the base scene; stop export if any entry is missing or duplicated.
- Keep the original uploaded scene unchanged. The base scene is held in memory only and is not uploaded or stored in the configuration.
- Clearly warn that generated scenes must be checked in M32-Edit and tested on the actual console before live use.
- Scene generation is separate from exporting/backing up the full service configuration.

## Storage and privacy

The app is a static browser application. Configuration data is stored locally in the browser for convenience, with explicit full-package export/import for backup and transfer between devices. Do not add a server or upload user scene files or performer information. Do not commit real scene files, actual performer details or venue-specific private information to this public repository.

## Delivery increments

1. Establish the versioned configuration data model, full import/export and browser-local persistence.
2. Build the reusable source library and service lineup with independent physical-input and M32 channel-strip mapping, mono/stereo support and validation.
3. Build the visual stage canvas with configurable stageboxes and sockets.
4. Build the direct stagebox XLR-to-M32 physical input mapping view and validation.
5. Implement M32 output mapping as a separate increment.
6. Implemented initial native .scn export for CH/Aux input-source assignments using a known-good base scene. Further routing and output-generation capabilities remain separate increments.

The initial output-mapping increment is implemented in UAT: destinations can be defined and M32 physical outputs mapped to them. Internal M32 source-to-output routing and native scene generation remain out of scope for that increment.

## Acceptance principles

- A library with more than 32 entries can be stored and exported even though no service can allocate more than the available desk channels.
- Changing a service lineup does not delete or overwrite unused library entries.
- Stagebox-to-physical-input mapping and M32 channel-strip-to-physical-input mapping are separate and can be configured independently.
- One physical input can feed multiple M32 channel strips.
- FX Returns 1–4 Left/Right are visible in channel mapping but cannot be assigned a physical input.
- Stereo physical-input allocations are validated; M32 channel mappings independently support channel strips CH 01–32 and Aux 1–6.
- Stagebox socket labels are editable and remain associated with their stagebox identity.
- Every stagebox XLR socket can be mapped directly to an M32 physical input in the supported input banks, with source-library name, stagebox/socket label and physical input visible together in the mapping view.
- Duplicate or conflicting physical-input assignments are clearly reported.
- Export then import round-trips the entire configuration without losing library entries, stageboxes, positions, names or assignments.
- Import errors leave the current workspace unchanged.
- No private scene or performer data is transmitted to a server.
