# Changelog

All notable changes to FlowSpec. Format roughly follows [Keep a Changelog](https://keepachangelog.com/). Versioning is loose pre-1.0 — minor bumps for feature batches, patch bumps for fixes.

## [0.17.1] — 2026-05-11

### Fixed
- **Spacebar (and `Z`) in text fields no longer steals focus or triggers pan / zoom mode**. The keydown handler correctly bails out when focus is in an `input` / `textarea` / `select`, so typing space in a label or description never set the `spaceDown` / `zKeyDown` flags. But the keyup handler was unconditionally calling `render()` on every release — which rebuilt the Properties panel from scratch and destroyed the textarea you were typing in. Fixed by only running `render()` when the corresponding state flag was actually set (i.e., when keydown actually fired the mode toggle). Same root cause whether you were editing a node label, a description, an edge label, or any text field elsewhere in the app

## [0.17.0] — 2026-05-11

### Added
- **Bend points are now first-class selectable objects**. Click a waypoint to select it (Shift-click toggles in/out). Selected waypoints render with a thicker selection-color stroke + a soft glow. Press <kbd>Delete</kbd> to remove all selected waypoints — multi-delete handles index shifts safely. Marquee select also picks up waypoints whose position is inside the box. Drag any selected waypoint to move the whole group. Right-click / long-press for color + delete still works
- **Frames are now true containers**:
  - The top of every frame is a slightly more opaque **title bar** that's the only place that selects + drags the frame
  - The frame's body is click-through (`pointer-events: none`) so you can **marquee-select inside large frames** like anywhere else on the canvas
  - Right-click on the title bar = frame menu; right-click in the body = canvas menu
- **Bend points respect frames** — when a frame moves, every waypoint whose position is inside the frame's bounds moves with it (folded into the same multi-drag set used for child nodes)
- **Faint dashed outline** around every node that belongs to a frame, in the frame's color, very low opacity. Visual cue for "what's in vs. out" without cluttering

### Changed
- Internal: `state.selectedWaypointIds` (Set of `"edgeId:idx"` keys), `state.drag.waypointKeys` + `origWaypointPositions` for unified drag

## [0.16.1] — 2026-05-11

### Fixed
- **Hotkey `0` now actually fires** when an edge is selected — added `ev.preventDefault()` and a status message confirming the bend was added. (If you had focus in an input field — e.g., the edge label — the keydown handler intentionally lets the keypress fall through to the input. Click the canvas first to defocus.)
- **Default waypoint dot is now visible** at any zoom / theme. Was using `currentColor` as the SVG fill fallback, which inherited a near-invisible value in some cases. Now the default fill comes from a CSS class (`var(--text)`) so dots are theme-aware and clearly visible whether the edge is selected or not. Unselected dots also bumped from r=4.5 to r=5

## [0.16.0] — 2026-05-11

### Added
- **Edge waypoints are now first-class visual handles** — much easier to discover and place:
  - **Click / tap a selected edge** anywhere on the line to drop a bend point at that spot. (First click selects the edge as before; the second click on the same edge bends it.)
  - **Hover affordance**: on desktop, hovering a selected edge shows a faint phantom waypoint dot under the cursor so you can see where the bend will land before clicking
  - **Hotkey `0`** drops a waypoint at the selected edge's current midpoint
  - **Always-visible bend dots** — waypoints render whether the edge is selected or not, so they read like discrete objects on the line
  - **Per-waypoint color** — right-click a bend dot for a 6-color palette + reset. Stored as `edge.waypoints[i].color` in the project file (backwards-compatible — old files load fine)
  - Lines stay straight (`L` segments) between waypoints — no curves
- Long-press on a waypoint (or the edge) on touch suppresses the deferred tap-to-add, so the context menu fires cleanly without accidentally bending the edge

### Changed
- Internal: `showContextMenu` now supports a `swatches` item type for inline color pickers in context menus

## [0.15.4] — 2026-05-11

### Changed
- **Selection highlight is now white in dark mode** (was the pastel green accent) — matches the dark-on-light feel of the light theme where selection is dark gray on a light background. Added a separate `--selection` CSS variable so the brand accent color (used for primary buttons + the dirty indicator + link text) stays the same
- Applies to: selected node stroke + glow, selected edge line + arrow, frame selection glow, frame resize corners, drag-to-connect handles, edge waypoint markers, alignment guides during drag, marquee rectangle, and connect-drag preview line

## [0.15.3] — 2026-05-11

### Fixed
- **Copy / Cut / Paste now includes frames** — copying a multi-selection that contains frames captures each frame plus every node inside it (even if those nodes weren't explicitly selected). Paste re-IDs frames and applies the same offset to nodes + edges + frames so the group lands together
- **Multi-select drag moves the whole group, including frames** — dragging any node or frame that's part of a multi-selection now translates every selected node, every selected frame, and every selected frame's children as one unit. Previously dragging a node only moved nodes, and dragging a frame in a multi-selection only moved that frame
- Unified drag state internally (`state.drag` now carries `nodeIds` + `frameIds`); frame `state.frameDrag` is now resize-only

## [0.15.2] — 2026-05-11

### Changed
- **Tools toggle (`⌃`) moved back into the header**, positioned just before the theme toggle (`🌙`). Collapsing the toolbar now hides the entire toolbar again (since the toggle lives elsewhere). The in-toolbar toggle from 0.15.1 didn't actually save much vertical room
- **Floating canvas toolbar reordered** to put the most common actions first: `+ Add ▾` → `⊡ Fit` → `⊙ Reset` → `▦ Tidy` → `⛶` fullscreen

## [0.15.1] — 2026-05-11

### Changed
- **Per-node-type buttons restored** in the top toolbar (desktop) alongside the dedicated Frame button — quick one-click adds without going through the dropdown. The floating `+ Add ▾` keeps the same content (8 types + Frame) and stays usable on every screen
- **Tools toggle moved from the header into the toolbar itself** (left edge). When you collapse the toolbar, every other group hides but the toggle stays visible so you can expand back. The `⌃` button in the header (between 🌙 and the right-side actions) is gone

## [0.15.0] — 2026-05-11

### Changed
- **Canvas toolbar consolidation** — the floating tools (previously only visible in fullscreen) are now **always visible** at the top-left of the canvas, on every screen and every mode. Contains `+ Add ▾` / `▦ Tidy` / `⊡ Fit` / `⊙ Reset` / `⛶` (fullscreen toggle)
- **Frame moved into the + Add menu** alongside the 8 node types (with the same `G` hotkey shown as a hint). No more separate Frame button
- **Top toolbar slimmed** — removed the per-node-type buttons (now in + Add), the mobile `+ Add` dropdown (redundant with floating), and the standalone `▦ Tidy`, `⊡ Fit`, `⊙ Reset`, `▢ Frame` buttons. What's left: Undo / Redo / Connect / Pan / Zoom / Issues / Delete
- **Single fullscreen toggle** — the header `⛶` button is gone. The floating `⛶` flips in / out of fullscreen and its title updates to match the current state. No more "Exit" button stranded in a corner
- All these consolidations stay reachable on every screen, including fullscreen and mobile, without overlapping the right-panel tabs

## [0.14.1] — 2026-05-11

### Changed
- **Fullscreen Exit button moved into the floating top-left toolbar** alongside the other fullscreen tools. On desktop it was overlapping the right-panel tab bar in the top-right corner; on mobile it was at the opposite corner from everything else. Now `+ Add ▾ / ▢ / ▦ / ⊡ / ⊙ / ⛶ Exit` are grouped together with a small visual gap before Exit

## [0.14.0] — 2026-05-11

### Added
- **Mobile marquee select** — long-press on empty canvas → context menu → `▭ Select region…` arms a one-shot marquee. Next finger-down on canvas starts the box at that point, drag expands it, lift finalizes. Mode auto-clears after one use
- **Tap-to-deselect on mobile** — a touch on empty canvas with no significant drag (< 5px) now clears the current selection, matching desktop click-empty behavior
- **Fullscreen floating mini-toolbar** — top-left now has a row of `+ Add ▾ / ▢ Frame / ▦ Tidy / ⊡ Fit / ⊙ Reset` buttons so you don't have to exit fullscreen for common actions

### Changed
- **Zoom minimum lowered to 5%** (was 20%). You can now zoom way out to see the whole layout of a big flow

### Fixed
- **Initial mobile centering** — on first load, mobile viewports often report a too-tall size while the browser chrome (URL bar, etc.) is still settling, so `fitToContent` landed slightly off. A second pass runs 200ms after init to re-center once layout has stabilized

## [0.13.0] — 2026-05-11

### Added
- **Roadmap** — new [ROADMAP.md](ROADMAP.md) listing future ideas (user-created templates, PWA install, share-via-URL, BYO-key LLM features, etc.)

### Changed
- **Origin marker is now visibly centered** on screen at default view. `Reset` and the initial load both pan the canvas so world (0, 0) sits at the viewport center, where the faint origin rectangle is now visible in the middle instead of clipped at the top-left
- **All built-in templates re-centered around (0, 0)** via a one-time `centerSeed()` pass at startup. Combined with the centered Reset behavior, templates now load nicely centered in the viewport
- **Open file / switch project / new project** all call `fitToContent()` after loading, so the project's content is automatically centered on the viewport regardless of where the nodes were saved

### Fixed
- **Template list overflow — permanent fix**. The `+ From template` dropdown was getting clipped by the modal's `overflow: hidden` and didn't scroll properly when many templates were added. Replaced the dropdown entirely with an in-modal **template gallery view**: clicking `+ From template…` swaps the modal body to a scrollable card list with a `← Back to projects` link. Add as many templates as we want — the body just scrolls

## [0.12.0] — 2026-05-11

### Added
- **Wrap selection in frame** — three ways to trigger:
  - `▢ Frame` toolbar button now sizes the new frame around the current selection when 2+ nodes are selected (drops a default empty frame otherwise)
  - Press `G` — same behavior
  - Right-click any selected node (when 2+ are selected) → **▢ Wrap N nodes in frame**
  - The new frame is sized to the bounding box of all selected nodes with 20px padding + 28px top space for the title
- **Origin marker** — a faint 800×600 rectangle with a small dot is always drawn at world (0, 0). Marks where `⊙ Reset` snaps you back to; ignorable when working, useful when navigating back
- **Frames in spec outputs**:
  - **Markdown** — new `## Groups` section listing each frame and its child node labels
  - **Narrative** — `Groups:` intro before the per-node prose, naming each frame's contents
  - **JSON** — top-level `frames` array with `id`, `title`, and `childNodeIds` (computed from spatial overlap at output time)
  - **Mermaid** — each frame becomes a native `subgraph "Title" … end` block. Nodes are defined inside their subgraph; free nodes outside; edges reference ids
  - Folder tree intentionally skips frames — that format is for file/folder structures, not groups

## [0.11.0] — 2026-05-11

### Added
- **Frames are now multi-selectable**, alongside nodes
- **Cmd/Ctrl + A** now selects all nodes AND all frames
- **Marquee select** (drag on empty canvas) now picks up frames whose center is inside the box, in addition to nodes
- **Shift-click a frame** to toggle it in/out of the current selection (matches the existing shift-click for nodes)
- **Delete / Backspace** now deletes every selected frame (was already true for the toolbar button)

### Changed
- Internal: `state.selectedFrameId` (single id) → `state.selectedFrameIds` (Set). All call sites updated; Properties panel still shows the dedicated single-frame view only when exactly one frame is selected and nothing else

## [0.10.5] — 2026-05-11

### Fixed
- Delete / Backspace key now deletes a selected frame. The Delete toolbar button already worked; the keydown handler had a stale guard that only fired for nodes / edges

## [0.10.4] — 2026-05-11

### Changed
- **Tidy now grows frames** to contain their original children. If a per-frame overlap pass pushes a child past the frame's edge, the frame's bounds expand (with 20px padding + room for the title) so nothing escapes its group. Frames only grow, never shrink — your manually-sized space is respected
- **Outer convergence loop** (max 8 iterations) — after frames grow they may now overlap their neighbors, so the algorithm re-runs the top-level overlap pass. Iterates until stable, then stops
- **Original-membership tracking** — children are remembered at the start of Tidy, not re-detected on each pass, so a pushed-out child is still moved with its frame and the frame still grows for it
- Status message now reports both kinds of work separately, e.g. "Tidied — 3 overlaps resolved, 1 frame grown"

## [0.10.3] — 2026-05-11

### Changed
- **Tidy is now overlap-resolution only** — no more re-layering. The layered BFS approach kept fighting the user's intent and drifted things off-screen on repeated runs. New behavior:
  - **Idempotent**: hitting Tidy twice does nothing the second time. If everything's already non-overlapping, nothing moves
  - **Frames move as units** — the frame rectangle and every node inside it translate together. Frames no longer drift away from their contents
  - **Per-frame pass** resolves overlaps among a frame's children after the top-level pass, so things inside a frame can shift apart without leaving the frame
  - **Smaller-overlap direction wins** — pushes by the minimum needed (20px padding, snapped to 10px). No teleporting across the canvas
  - **Bounded at 50 iterations** so cascade-pushing can't run away
- Scope unchanged: selection-only when something is selected, otherwise all top-level objects (frames as units + un-framed nodes) plus a per-frame pass

### Removed
- Layered BFS layout, barycentric ordering, width-aware layer placement, centroid-recentering — all replaced by the simpler overlap-resolution algorithm. Templates and the example flow still render correctly because they already have sensible positions

## [0.10.2] — 2026-05-11

### Changed
- **Auto-layout is now scoped + frame-aware** so it stops trampling carefully-arranged flows:
  - If anything is selected, Tidy operates **only on the selection** — useful for laying out a sub-graph without disturbing the rest
  - If nothing is selected, Tidy skips every node that's currently inside any frame. Frames are treated as manually-laid-out regions
  - Confirms before tidying > 80 nodes when nothing is selected
- **Width-aware spacing** — horizontal placement uses each node's actual width plus a 40px gutter, so wide / Large / long-described nodes no longer overlap their neighbors
- **Tall-aware spacing** — vertical placement uses the tallest node in each layer
- **Centered on original position** — the laid-out block is centered on the current centroid of the target nodes, so things stay near where they were instead of teleporting
- **Collision-resolve pass** — after placement, any remaining overlapping bounding boxes get nudged apart (bounded at 20 iterations)

## [0.10.1] — 2026-05-11

### Fixed
- Auto-layout (`▦ Tidy` / `L`) no longer freezes the tab on flows with cycles or back-edges. The longest-path BFS now caps per-node depth updates at N (total node count), guaranteeing termination at O(N²) max while preserving the layered layout

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
