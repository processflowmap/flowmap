# Changelog

All notable changes to FlowMap are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versioning follows
[SemVer](https://semver.org/).

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
