# Changelog

All notable changes to FlowSpec. Format roughly follows [Keep a Changelog](https://keepachangelog.com/). Versioning is loose pre-1.0 — minor bumps for feature batches, patch bumps for fixes.

## [0.10.0] — 2026-05-11

### Added
- **Generate with AI** modal (`⋮ More → 🤖 Generate with AI…`). Step 1 copies a paste-ready prompt containing FlowSpec's full schema + layout rules. Step 2 parses the LLM's JSON response and loads it as a new project (or replaces current). Markdown code fences are stripped automatically; auto-layout runs if positions are missing
- "or generate a flow with AI ✨" link in the Properties tab's empty-state for discoverability
- `⋮ More` menu now visible on desktop too (was mobile-only), with Find & Replace and Generate with AI as the non-duplicate entries

## [0.9.0] — 2026-05-11

### Added
- **Issue suppression** — each entry in the issues panel has a `×` to suppress it. Suppressed entries appear in a separate section with a `↺` to bring them back; "Clear all" wipes the list. Survives reload, Save/Open, and undo
- Toolbar `⚠` button stays accessible whenever any suppressions exist (even if all are suppressed), so the panel can always be reached
- Orphaned suppressions (the underlying node was deleted or the issue no longer triggers) appear faded in the panel with a "no longer flagged" tag

### Fixed
- Login flow + Data pipeline templates no longer trigger dead-end warnings on import (added missing End nodes)
- Dropdown menus (especially `+ From template`) now scroll inside themselves when content overflows the viewport

## [0.8.0] — 2026-05-11

### Added
- **Find & Replace** modal (`Ctrl + H` / `Cmd + Shift + H`). Scope checkboxes for labels, descriptions, edge labels. Match-case toggle. Find reports node + edge match counts; Replace All snapshots for undo
- **Edge waypoints** — right-click an edge → "Add waypoint here" inserts a handle on the closest segment. Drag the handle to bend the edge. Right-click a handle → Delete; right-click the edge → "Clear waypoints"
- **Frames / groups** — `▢ Frame` toolbar button drops a labeled container. Drag the body to move; nodes whose centers are inside the frame's current bounds move with it. Corner handles to resize. Frame Properties: title + color + delete
- **Onboarding tour** — 7-step first-launch walkthrough with spotlight + popover. Targets adapt to layout (desktop vs. mobile). "Restart the welcome tour" button in the Help tab can re-launch anytime

## [0.7.0] — 2026-05-11

### Added
- **Flow validation** — runs on every change, flagging unlabeled decision branches, dead-ends, orphan nodes, unreachable cycles, and missing labels. Inline ⚠ badges on flagged nodes; toolbar `⚠ N` button opens a floating issues panel with click-to-jump
- **Edge styling** — per-edge style (solid / dashed / dotted) and color picker (6 path-meaning colors + reset) in the edge Properties
- **Custom node colors** — per-node fill override via an 8-color palette + reset in node Properties
- **Self-loops** — `source === target` edges now render as a small arc above the node instead of a zero-length line

## [0.6.0] — 2026-05-11

### Added
- **Auto-layout** (`▦ Tidy` or `L`) — layered BFS from root nodes with barycentric ordering to minimize edge crossings; fits to view after
- **Connect by drag** — selecting a single node shows 4 small handles; drag any to another node to create a connection. Preview line follows the cursor
- **Right-click context menus** (desktop) / **long-press menus** (touch) on nodes, edges, and the canvas — Edit / Duplicate / Copy / Cut / Paste here / Connect / Delete
- **Recent files** — Projects modal groups linked projects under "Recent files" at the top; browser-only projects below
- **Project templates** — `+ From template ▾` dropdown with Login flow, Data pipeline, File / folder structure, Request with retry, Decision tree starters
- **Quick-add hotkeys** — press 1–8 to add the corresponding node type
- **Multi-line node labels** — Label field is now a textarea; press Enter for line breaks. Labels render across multiple lines in SVG
- **Alignment guides** — dragging a single node shows dashed snap lines when its edges or center align with another node (within 6px screen); snaps to the matching position
- **Floating + Add menu** in fullscreen mode (top-left) so you can keep adding nodes without leaving fullscreen
- **Copy / cut / paste** (`Cmd/Ctrl + C / X / V`) on selected nodes; internal connections come along on copy. Right-click → Paste here lands at the click position
- "Restart welcome tour" button at the bottom of the Help tab

### Fixed
- Right-panel `›` collapse button now actually toggles (was one-way). Icon flips between `›` and `‹`
- Help tab reorganized into 10 collapsible `<details>` sections with a dedicated Keyboard shortcuts table

### Changed
- Full README rewrite to reflect the current feature set; dropped LLM-specific framing for tool-agnostic wording

## [0.5.0] — 2026-05-11

### Changed
- Renamed `flowspec.html` → `index.html` so GitHub Pages serves the bare URL
- Added `noindex, nofollow` meta tag to keep the deployed site out of search engines

## [0.4.0] — 2026-05-11

### Added
- **Mobile / touch support** — Pointer Events refactor across all drag, pan, drop, marquee, edge-hit, and node-hit handlers. Mouse maps 1:1 so desktop behavior is unchanged
- **Pinch-to-zoom + two-finger pan** anchored on the midpoint between fingers
- Responsive layout at ≤ 800px: header wraps to a compact single row; right panel becomes a bottom slab (50dvh max) that collapses to a 44px tab strip; project-action buttons always visible (no hover on touch)
- `@media (pointer: coarse)` sizing: 40px+ buttons, 16px form fields to suppress iOS focus-zoom
- **Fullscreen mode** (`⛶`) hides header + toolbar entirely. Floating exit-fullscreen button stays in the top-right
- **Tools collapse** (`⌃`) hides just the toolbar; header stays visible. Defaults to collapsed on mobile
- **Compact mobile header**: `⋮ More` menu groups Spec / Projects / Open / Save / Save As / Export-JSON / Export-PNG
- **+ Add dropdown** on mobile holds the 8 node-type buttons
- Icon-only toolbar buttons at narrow widths (Connect, Pan, Fit, Reset, Delete shed text)
- Selection glow on selected node shape (heavier in dark mode), wider edge hit area on touch, larger diamond bounding box with description-aware text wrapping
- Tagline: "visual → LLM text" → "visual → spec"

## [0.3.0] — 2026-05-11

### Added
- **Document-style file model** — Open / Save / Save As behave like a real editor. Each project can be linked to a `.flowspec.json` file via File System Access API (Chrome / Edge); falls back to download dialog elsewhere
- **Linked-file chip** next to the title shows the linked filename; click `×` to unlink
- **Dirty indicator** (`•`) — appears when in-memory state differs from the linked file; cleared on Save / Save As. Also shown in the browser tab title
- Save button in the Spec tab — exports the current format (md / txt / json / mmd) with the right extension; opens dialog in the linked file's folder

### Removed
- Workspace folder concept (was confusing alongside Export JSON). Replaced by per-project linked files
- "Save to disk" / "Export JSON" / "Import JSON" header buttons (consolidated into Save / Open / Export menu)

### Changed
- Dropped Claude-specific phrasing from the Markdown format description

## [0.2.0] — 2026-05-11

### Added
- **Zoom** — hold `Z` and scroll, or `Ctrl/Cmd + scroll`, or pinch on a trackpad. Cursor-anchored, 20% – 300%
- Toolbar zoom controls: `−` / current% / `+`, `⊡ Fit`, `⊙ Reset`
- **Dark mode** (default) with light theme toggle; CSS variables drive every color
- **Spec output as a tab** — moved from a fixed bottom panel into the right panel alongside Properties and Help. Output panel collapse → right-panel collapse (`›` button, icon rail on desktop)
- **Per-node sizing** (Small / Medium / Large) saved with the project
- Larger node defaults, multi-line description preview (up to 2 lines)
- Search nodes (`Cmd/Ctrl + F`) — searches labels + descriptions
- Duplicate (`Cmd/Ctrl + D`), arrow-key nudge (Shift = bigger step), Fit-to-content (`F`)
- PNG export of the chart
- File System Access API integration (initial version)

## [0.1.0] — 2026-05-10

### Added
- Single-file visual flowchart builder
- 8 node types — Start, Process, Decision, End, Folder, File/Data, Screen, Note
- 5 output formats — Structured Markdown, Narrative, Folder tree (ASCII), JSON, Mermaid
- Multi-project workspace auto-saved to localStorage
- Drag-and-drop nodes, two-click connect, edge labels
- Multi-select via marquee or Shift-click, with align + distribute
- Pan canvas (Space + drag), undo / redo (50 steps)
- Zero dependencies; pure HTML / CSS / vanilla JS
