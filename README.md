# FlowSpec

**visual → spec**

A single-file flowchart builder that turns visual diagrams into clean structured text. Build app flows, decision trees, data pipelines, or file structures, then export the spec as Markdown, narrative prose, an ASCII folder tree, JSON, or Mermaid. Runs in any modern browser, works offline, dark mode by default.

> No installation. No build step. No dependencies. One HTML file.

Live demo: [https://psychedelicrobotsquid.github.io/FlowSpec/](https://psychedelicrobotsquid.github.io/FlowSpec/)

---

## Why

Most diagramming tools (Excalidraw, draw.io, Whimsical) export images or proprietary formats. When you want to *describe* a flow — to a teammate, to a doc, to an AI, to your future self — you end up writing it out by hand. FlowSpec inverts that: lay it out visually, copy clean text out.

---

## Features

### Editor
- **8 node types** — Start, Process, Decision, End, Folder, File/Data, Screen, Note — each with distinct shapes and colors
- **Per-node sizing** (Small / Medium / Large), labels (multi-line), descriptions, and custom color overrides
- **Edge styling** — solid / dashed / dotted, custom color, waypoints to bend connections (click an already-selected edge to drop a bend point, or press `0` to drop one at the midpoint). Bend points are first-class: click to select, Shift-click to multi-select, drag to move the group, `Delete` to remove, right-click for color
- **Frames / groups** — labeled containers around a set of nodes. **The entire perimeter** (top title strip + slim left / right / bottom borders) is the drag handle — grab any side to move the frame; everything inside (nodes + bend points) moves with it. The frame's body is click-through, so you can **marquee-select inside large frames**. Resize from any of the 8 anchors (4 corners + 4 side midpoints). Faint dashed outline in the frame's color shows which nodes belong to it. Press `G` (or click `▢ Frame`) with 2+ nodes selected to wrap them. Multi-selectable; included in spec output (Markdown / Narrative / JSON / Mermaid)
- **Drag-to-connect** — drag from handles on a selected node to any other node
- **Multi-select** — marquee drag or Shift-click, with align + distribute tools
- **Copy / cut / paste / duplicate** (Cmd/Ctrl + C / X / V / D), works across projects
- **Undo / redo** (Cmd/Ctrl + Z) — up to 50 steps
- **Quick-add hotkeys** — press 1–8 to add a node of that type
- **Alignment guides** — dashed snap lines appear when you drag a node near another's edges or center
- **Tidy** (`▦` or `L`) — overlap resolution that pushes nodes / frames apart by the minimum needed. Idempotent, scoped to selection if any, frames move as units and grow to contain their children
- **Right-click / long-press** context menus on nodes, edges, and the canvas
- **Double-click a node or frame** to open Properties and jump straight to its label for editing
- **Search** (Cmd/Ctrl + F) — find nodes by label or description
- **Find & Replace** (Ctrl + H / Cmd + Shift + H) — bulk edit labels, descriptions, or edge labels
- **Flow validation** — flags unlabeled decisions, dead-ends, orphans, unreachable nodes. Suppress per-warning, per-node, or globally — see [Validation & errors](#validation--errors) for the full warning catalog

### Canvas
- **Pan** — hold Space + drag (or one-finger drag on touch)
- **Zoom** — hold Z + scroll, Ctrl + scroll, or pinch on a trackpad / touchscreen. 5% – 300%
- **Fit-to-content** (`⊡` or `F`) and Reset view (`⊙`)
- **Fullscreen** mode (`⛶`) hides the header + top toolbar; the floating canvas toolbar (`+ Add ▾ / ⊡ / ⊙ / ▦ / ⛶`) stays at the top-left so common actions are always one click away
- **Faint origin marker** at world (0, 0) — visible always at low opacity so Reset is predictable

### Files
- Each project can be **linked to a `.flowspec.json` file on disk** — Open, Save, Save As behave like a real document editor (Cmd/Ctrl + O / S / Shift+S)
- **📋 Import JSON** — paste a `.flowspec.json` payload directly from your clipboard, no file needed. Useful when someone shared a flow with you in a chat or email, or when you have JSON from a previous AI conversation. Reachable from the header button, the Projects modal's third "Create new" card, or the mobile `⋮ More` menu
- **Linked-file chip** next to the title shows the link; **• dirty indicator** when in-memory differs from disk
- **Auto-save to localStorage** runs in the background — a crash-safety net independent of disk saves
- Multi-project workspace; switch between projects without losing any state
- **5 project templates** to start from: Login flow, Data pipeline, File / folder structure, Request-with-retry, Decision tree

### Spec output (the point of the tool)
- **5 formats** in the Spec tab: Structured Markdown, Narrative, Folder tree (ASCII), JSON, Mermaid
- **Copy** to clipboard or **Save…** the spec to a file — when the project is linked, the save dialog opens next to the `.flowspec.json`

### Export
- **JSON copy** — download a `.flowspec.json` without changing the linked file
- **PNG image** — rasterized chart for docs, Slack, presentations

### Look & feel
- **Dark mode by default**, light theme one click away
- **Mobile-friendly** — pointer events, pinch + two-finger pan, responsive layout with a compact 1-row header and bottom-drawer side panel
- **Keyboard-first** — everything has a shortcut
- **Selection glow** so the active node is unmistakable in dark mode
- **Onboarding tour** for first-time visitors, replayable from the Help tab

### Generate with AI
Click **🤖 Generate** in the header (or, on mobile, `⋮ More → 🤖 Generate with AI…`) to bring up the prompt builder. It hands you a ready-to-paste prompt that contains FlowSpec's full schema + layout rules. Drop it into any LLM (ChatGPT, Claude, Gemini, etc.), describe what you want, paste the JSON back, click Load. No file save / load step needed. Optionally include your current flow so the model can extend or refactor it.

### Zero dependencies
Pure HTML, CSS, vanilla JavaScript. Single file. Works offline. No tracking, no analytics, no network calls.

---

## Output formats

| Format | What it's good for |
|---|---|
| **Structured Markdown** | The default. Clean hierarchy, all nodes and connections enumerated, plus an ASCII tree of the flow. Best for documentation or AI prompts. |
| **Narrative** | Prose per node ("Authenticate: POST /auth, store token. Leads to: Show dashboard."). Good when intent matters more than structure. |
| **Folder tree (ASCII)** | `tree`-style folder layout. Drop in Folder and File/Data nodes, connect parents → children, get clean directory output. |
| **JSON** | Machine-readable, no layout info. Feed into scripts or have an LLM generate code from the graph. |
| **Mermaid** | Standard `flowchart TD` syntax. Paste into GitHub READMEs, Notion, or any Mermaid renderer. |

---

## File format

The project file (`.flowspec.json`, also produced by the header **⤓ Export → JSON copy**) is the round-trip format — it preserves positions, sizes, descriptions, edge IDs, and any other state needed to reopen the project exactly as you left it.

The **Spec tab's** JSON format is intentionally minimal (just `id` / `type` / `label` / `description` / edges) and is meant to be machine-readable text for downstream tools or AI prompts, not a backup.

---

## Validation & errors

FlowSpec watches your flow for **structural problems while you build** — the kinds of issues that turn into *"wait, what's supposed to happen here?"* when someone (or some LLM, or your future self) actually reads the spec. A **⚠** badge in the header counts active issues, each affected node gets a small **!** corner marker, and clicking the badge opens the **Flow issues** panel with the full list.

Common things it catches:

- A **Decision** diamond with only one branch coming out of it — almost always a bug, since if there's nothing to choose between, it isn't a decision
- A **Decision** with multiple branches but no labels — readers can't tell which path is *yes* vs *no* vs *retry*
- **Dead-end nodes** that aren't actually End nodes — the reader arrives and has no idea what should happen next
- **Orphan nodes** floating with no connections at all — usually leftover from restructuring you forgot to clean up
- **Unreachable nodes** that have incoming edges but can't be reached from any starting point (a cycle without an entry)
- **Untitled nodes** — placeholders you set down and never came back to fill in

The point is to catch these *before* you hand the spec off — to a teammate, to an LLM that's going to write code from it, to a doc, to your future self three months from now when you can't remember why you drew it this way. Validation is **opt-out**: turn it off entirely when you're sketching, leave it on when you want a clean, complete spec.

### How to manage warnings

Three levels of suppression, from most → least surgical:

1. **Per-warning** — click the `×` on any single row in the Flow issues panel. Suppresses just that one combination of (node + warning text). If you fix the underlying issue and reintroduce it later, the suppression remembers and re-applies.
2. **Per-node** — open Properties for a node and tick **Suppress warnings for this node**, or right-click the node → **Suppress warnings**. Hides every warning targeting that node, no matter the type. Use when a node is intentionally a dead-end / orphan / etc.
3. **Globally** — toggle **Hide all flow validation** at the top of the Flow issues panel, or right-click the **⚠** badge → **Hide all flow issues**. Mutes the badge, the per-node `!` markers, and the panel entirely. Per-project setting — persists in the file. Use while sketching; turn it back on when you're tightening up.

### Warning types

| Severity | Message | What it means | Why you might silence it |
|---|---|---|---|
| 🔴 error | **Decision has no outgoing branches** | A diamond Decision node has zero outgoing connections — there's nothing to branch into. | You haven't drawn the branches yet, or the decision is intentionally a placeholder. |
| 🟡 warning | **Decision has only one branch** | A Decision node has a single outgoing connection — there's nothing being decided. | You're modeling a guard / pre-condition rather than a true branch. |
| 🟡 warning | **Decision has N unlabeled branches** | Two or more outgoing edges from a Decision, but the edges have no labels. | The branch labels aren't useful for what you're documenting. |
| 🟡 warning | **Dead end — no outgoing connection** | A flow node (Process / Decision / Screen / Start) has nowhere to go. End / Folder / Data / Note nodes are exempt — see [Node-type behavior](#how-warnings-interact-with-node-types) below. | The flow genuinely terminates here and you don't want to drop in an End node. |
| 🟡 warning | **Orphan — not connected to anything** | A flow node with no incoming and no outgoing connections. Folder / Data / Start / Note are exempt. | A node you parked while restructuring. |
| 🟡 warning | **Unreachable from any starting node** | The node has incoming edges but you can't get to it from any root (it's behind a cycle with no entry). Folder / Data / Note are exempt. | Intentional cycle without a clear start, e.g. a state machine without an explicit "init". |
| 🔵 info | **Node has no label** | The node text is empty or `(untitled)`. Applies to every node type. | Placeholder while you decide what it should be called. |
| 🔵 info | **No clear starting node** | Every node has at least one incoming connection — there's no obvious entry point. Flow-level only (no specific node). | The flow loops in a way that's intentional, or it's a fragment meant to be embedded. |

### How warnings interact with node types

Not every warning applies to every node type. The validator divides node types into two camps and treats them differently:

- **Flow nodes** — `start`, `process`, `decision`, `end`, `screen` — model **procedural flow**. They get the full set of structural warnings: dead ends, orphans, unreachability, etc. Every node in a procedural flow is expected to be reachable from a Start and either lead somewhere or be an explicit End.
- **File-structure nodes** — `folder`, `data` — model **hierarchy**. There's no "Start node" for a file system; the root folder *is* the start. Leaf folders are normal. Brand-new folders are normal. Disconnected sub-trees (multiple top-level dirs) are normal. So these types are exempt from every structural warning. The only thing they can ever trip is the info-level "Node has no label."
- **Note** is exempt from everything except "no label" — notes are commentary, not part of the flow at all.

The full matrix:

| Warning | start | process | decision | end | folder | data | screen | note |
|---|---|---|---|---|---|---|---|---|
| Decision has no/one/unlabeled branches | — | — | ✅ | — | — | — | — | — |
| Dead end | ✅ | ✅ | ✅* | — | — | — | ✅ | — |
| Orphan | — | ✅ | ✅ | ✅ | — | — | ✅ | — |
| Unreachable | ✅ | ✅ | ✅ | ✅ | — | — | ✅ | — |
| No label | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

\* For Decisions the dead-end is reported as the more specific "Decision has no outgoing branches" (error), not the generic "Dead end" (warning).

#### What this means for mixed flows

If your flow passes through a Folder or Data node — for example: `Process: "Save user data" → Data: "user.json"` — the chain is allowed to terminate at the Data without an explicit End. *"Save to disk"* and *"move to folder"* are legitimate end states; the validator doesn't pester you to wrap them with an End node.

The trade-off: if you had a procedural flow that you intended to terminate at an explicit End and you accidentally left it ending at a Data, the validator can no longer catch that. In practice that nudge was wrong more often than right — most "save to file" flows really do end at the file — so it's gone. If you want explicit End nodes everywhere, add them deliberately.

#### Why this matters for the spec output

This treatment also means your **folder-tree exports** are quiet — you can drop a Folder, immediately switch the Spec format to *Folder tree*, and get a clean directory output without "Dead end" / "Orphan" / "Unreachable" noise on every leaf. Same with file-structure JSON or Mermaid output.

---

## Keyboard shortcuts

| Key | Action |
|---|---|
| `1`–`8` | Add a node of that type |
| `0` | Add a bend / waypoint to the selected edge (at midpoint) |
| `C` | Toggle Connect mode |
| `F` | Fit to content |
| `L` | Auto-layout (Tidy) |
| `G` | Wrap selection in frame (or add empty frame) |
| `Z` + scroll | Zoom in / out (cursor-anchored) |
| `Ctrl` + scroll *(or pinch)* | Zoom in / out |
| `Space` + drag | Pan canvas |
| Arrow keys | Nudge selected nodes (Shift = bigger step) |
| `Delete` / `Backspace` | Delete selection |
| `Esc` | Cancel mode / deselect / close menu |
| `Cmd/Ctrl` + `A` | Select all |
| `Cmd/Ctrl` + `C` / `X` / `V` | Copy / Cut / Paste |
| `Cmd/Ctrl` + `D` | Duplicate |
| `Cmd/Ctrl` + `Z` / `Shift+Z` | Undo / Redo |
| `Cmd/Ctrl` + `F` | Search nodes |
| `Ctrl` + `H` / `Cmd` + `Shift` + `H` | Find & Replace |
| `Cmd/Ctrl` + `O` | Open file… |
| `Cmd/Ctrl` + `S` / `Shift+S` | Save / Save As… |

---

## Mobile gestures

| Gesture | Action |
|---|---|
| 1-finger drag on canvas | Pan |
| 1-finger drag on a node | Move the node |
| Tap a node / edge | Select |
| Tap empty canvas | Deselect |
| 2-finger pinch | Zoom |
| 2-finger drag | Pan + zoom together |
| Long-press (½ sec) | Open context menu (includes **Select region…** to draw a marquee with one finger) |

---

## Run it locally

Just open `index.html` in your browser — that's the whole app.

```bash
git clone https://github.com/PsychedelicRobotSquid/FlowSpec.git
cd FlowSpec
# Then open index.html in your browser, or:
python3 -m http.server 8000   # serve at http://localhost:8000
```

No build, no install. Your projects are stored in browser `localStorage` and (optionally) in `.flowspec.json` files on disk that you choose.

---

## Hosting

To share with friends, deploy as a static site:

- **GitHub Pages** — Settings → Pages → Deploy from branch `main` / root → done. Live within ~30 seconds.
- **Netlify / Cloudflare Pages / Vercel** — drag-and-drop the file, or connect this repo. All free.

The app is one HTML file, so anywhere that hosts static files works.

---

## Browser support

- **Chrome / Edge** — full feature set, including in-place file saving via the File System Access API
- **Firefox / Safari** — everything works, file save / open fall back to download / file-picker dialogs
- **iOS Safari** — pinch, pan, drag, full editor; file save = browser download

---

## Tech notes

- Single HTML file, ~6000 lines including CSS and JS
- SVG-based canvas; positions stored as world coordinates with separate pan/zoom
- Auto-saves to `localStorage` under `flowspec.workspace.v1`; per-project file handles in IndexedDB
- Pointer Events throughout — one input model for mouse, touch, stylus
- Theme via CSS variables — separate `--selection` color so the highlight reads cleanly on both light and dark backgrounds

---

## What's coming

See [ROADMAP.md](ROADMAP.md) for ideas on the runway — user-created templates, PWA install, share-via-URL, BYO-key LLM features, and more.

## License

MIT — do whatever you want with it.
