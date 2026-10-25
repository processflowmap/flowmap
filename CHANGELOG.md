# Changelog

All notable changes to FlowMap are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versioning follows
[SemVer](https://semver.org/).

## [1.2.4] - 2026-10-25

Cross-view routing stability and strict orthogonal Square geometry.

### Fixed

- **Matrix and Hybrid no longer reuse Canvas waypoints** — custom absolute
  coordinates from Canvas previously produced long diagonal cross-lane routes
  after automatic relayout. These views now always derive fresh automatic Square
  geometry while leaving the saved Canvas override untouched.
- **Custom leads remain orthogonal after endpoint movement** — the first and last
  interior waypoints realign with current source/target lead heights while the
  user's interior channel stays fixed.
- **Diagonal Square routes are rejected** — malformed custom geometry falls back
  to safe local Square geometry without deleting document metadata or mutating
  history during rendering.

### Changed

- Custom route selection and segment dragging are Canvas-only. Matrix and Hybrid
  explain that a saved Canvas custom route will return; Above/Below remain
  available and generate fresh geometry.
- Switching views refreshes Edge properties so controls always match the current
  layout and effective route behavior.
- Added Matrix and Hybrid routing-stability screenshots and explicit all-view
  orthogonal-segment verification for every embedded template.
- `APP_VERSION` bumped to 1.2.4; service-worker cache bumped `flowmap-v6` →
  `flowmap-v7` (deploy requirement).

## [1.2.3] - 2026-10-25

Persistent routing controls and stronger deterministic Square optimization.

### Added

- **Draggable Square segments** — drag horizontal middle segments vertically or
  vertical middle segments horizontally. Movement snaps to the 10-unit grid,
  preserves fixed source/target leads and rounded corners, and creates persistent
  custom interior waypoints.
- **Selected-edge route controls** — choose Auto / Above / Below / Custom, adjust
  signed channel offsets in 20-unit steps, or Reset to automatic. Custom routes
  participate in undo/redo, autosave, and JSON export/import; schema-1 documents
  without route metadata remain automatic.
- **Expanded automatic routing** — forward midlines at 25% / 50% / 75%, more
  obstacle-boundary channels, four deterministic edge orders, and bounded
  whole-graph refinement keep the lowest-scoring result.
- **Routing-control screenshot** — shows a selected custom route, waypoint
  handles, and the new Edge properties controls.

### Changed

- Custom routes stay fixed while automatic routes optimize around them. Route
  preferences and waypoints invalidate the geometry cache; segment movement
  recomputes only the dragged edge and full optimization runs on release.
- Collision scoring now exempts only mandatory endpoint leads, not every segment
  crossing an endpoint block's interior. Undo/redo refreshes the property panel
  so its controls always reference the restored document objects.
- Candidate breadth and refinement passes are bounded more tightly on dense
  graphs to protect editing performance.
- Curved routing, pending connections, themes, templates, decimals, PNG export,
  offline behavior, and the document schema remain compatible.
- `APP_VERSION` bumped to 1.2.3; service-worker cache bumped `flowmap-v5` →
  `flowmap-v6` (deploy requirement).

## [1.2.2] - 2026-10-25

Obstacle-aware Square routing with deterministic crossing reduction.

### Added

- **Candidate-based Square router** — backward and obstructed forward connections
  compare local, obstacle-derived, and wider above/below channels. Unrelated-block
  avoidance takes priority over edge crossings, bends, and path length.
- **Whole-graph refinement** — a deterministic second pass reconsiders each route
  against the other selected edges. Shared endpoints and collinear overlap are
  not counted as ordinary crossings.
- **Routing-focused screenshot** — documents obstacle avoidance and clearer
  backward channels alongside the refreshed application references.

### Changed

- Labels and selected-edge delete controls use distance along the chosen route.
- Geometry is cached by style, mode, block bounds, and endpoints. Active drags use
  lightweight obstacle avoidance, with full crossing refinement on release.
- Curved routing, pending connections, document schema, and stored documents are
  unchanged; generated route geometry is never persisted.
- `APP_VERSION` bumped to 1.2.2; service-worker cache bumped `flowmap-v4` →
  `flowmap-v5` (deploy requirement).

## [1.2.1] - 2026-10-25

System-aware dark mode and a quieter loop presentation.

### Added

- **System, Light, and Dark appearance** — the Theme menu follows live OS changes
  by default and stores manual choices separately in `flowmap.v1.theme`. The
  complete interface, SVG renderer, and PNG export use the active resolved theme.
- **Restyled loops** — loop blocks use a subtle amber-tinted fill and small SVG
  circular-arrow marker instead of a dashed outline. Criticality borders,
  selection, tracing precedence, and loop tooltips remain intact.

### Changed

- Cycle edges are solid amber rather than dashed; temporary interaction overlays
  retain their existing dashed treatment.
- What's New, README, and six light/dark screenshots updated.
- `APP_VERSION` bumped to 1.2.1; schema and local-storage keys remain unchanged.
- Service-worker cache bumped `flowmap-v3` → `flowmap-v4` (deploy requirement).

## [1.2.0] - 2026-10-24

Offline starting points and realistic fractional resource allocation.

### Added

- **File menu** — New blank map, New from template…, Import JSON, Export JSON,
  and Export PNG now share one compact, keyboard-accessible dropdown.
- **Embedded template infrastructure** — a variable-length curated catalog and
  simple picker work entirely offline, with no runtime template requests.
- **Safe document replacement** — blank maps and templates share one pipeline
  that deep-clones and validates the candidate, confirms replacement, resets
  selection and history, synchronizes every view, autosaves, and fits non-empty
  content. Failed or cancelled loads leave live state untouched.
- **Decimal resources** — any finite positive resource quantity is accepted and
  stored without rounding in the property panel, full Table, and data drawer.

### Changed

- Resource inputs use `step="any"` and one shared parser. Blank stores `null`;
  zero, negative, non-numeric, and non-finite inputs restore the previous value
  without a mutation or extra undo entry.
- The first-run demo now includes a realistic fractional resource allocation.
- What's New, README, template contribution guidance, and screenshots updated.
- `APP_VERSION` bumped to 1.2.0; schema and local-storage key remain unchanged.
- Service-worker cache bumped `flowmap-v2` → `flowmap-v3` (deploy requirement).

## [1.1.0] - 2026-10-24

Data-visibility layer: the data behind blocks (owners, stacks, effort) is now
visible without opening the property panel.

### Added

- **Metadata badges on blocks** — owner team and software stack render as small
  muted lines beneath the block label in Canvas mode. Blocks shorter than 70px
  (e.g. terminators) show one combined line so nothing clips. Toggle with the
  **Badges** toolbar button; persisted per document, on by default.
- **Data table drawer** — a collapsible, abridged, editable table docked at the
  bottom of Canvas/Matrix/Hybrid views: #, Label, Owner team, Software stack,
  Criticality, Man-hours, Resources, Time to complete. Edits commit through the
  same code path as the full Table view, so the canvas updates live. Toggle via
  the grabber handle, the **Table ▾** toolbar button, or the `B` key.
  Hidden in Table mode (redundant there); collapsed by default.
- What's-new card entry, keyboard-shortcuts sheet entries (`B`, Badges), and a
  first-run hint line covering both features.

### Changed

- Table rendering refactored into a single builder serving both the full Table
  view (all 14 columns) and the drawer (abridged 8 columns).
- Drawer table rebuilds only on document mutations — never on pan/zoom — and
  the stage bottom inset always equals the drawer's rendered height, so the
  drawer pushes content up instead of overlaying it.
- Service-worker cache bumped `flowmap-v1` → `flowmap-v2` (deploy requirement).
- `APP_VERSION` bumped to 1.1.0 (was still 1.0.0 after the v1.0.1 label).

## [1.0.1] - 2026-10-18

### Fixed

- `og:image` meta tag now uses an absolute URL so social preview cards render.

## [1.0.0] - 2026-10

Initial public release.

- Four live views of one document: freeform Canvas, Matrix (any two properties
  as axes, drag-to-reassign), Hybrid (strict Y lanes + repeating flow-ordered X
  bands), and an all-properties editable Table.
- Five block types with criticality color-coding; owner team / software stack
  dropdowns that grow as new values are typed.
- Effort tracking: man-hours and assigned resources with calculated
  time-to-complete.
- Orthogonal (square) edge routing by default, curved bezier optional; edge
  labels; self-loop/duplicate guardrails; cycle detection (Tarjan SCC) with
  amber dashed warning.
- Hover flow tracing (upstream blue / downstream green), multi-select +
  marquee, 100-step undo/redo, Ctrl+D duplicate, collapsible property panel.
- Autosave to localStorage, JSON export/import, PNG export, demo dataset on
  first run.
- Installable PWA: cache-first service worker, manifest, icons; works fully
  offline, including from `file://`.
