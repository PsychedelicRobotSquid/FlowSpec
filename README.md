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
- **Edge styling** — solid / dashed / dotted, custom color, waypoints to bend connections
- **Frames / groups** — labeled containers around a set of nodes; drag a frame to move it and everything inside. Press `G` (or click `▢ Frame`) with 2+ nodes selected to wrap them in a sized frame. Multi-selectable; included in spec output (Markdown / Narrative / JSON / Mermaid)
- **Drag-to-connect** — drag from handles on a selected node to any other node
- **Multi-select** — marquee drag or Shift-click, with align + distribute tools
- **Copy / cut / paste / duplicate** (Cmd/Ctrl + C / X / V / D), works across projects
- **Undo / redo** (Cmd/Ctrl + Z) — up to 50 steps
- **Quick-add hotkeys** — press 1–8 to add a node of that type
- **Alignment guides** — dashed snap lines appear when you drag a node near another's edges or center
- **Tidy** (`▦` or `L`) — overlap resolution that pushes nodes / frames apart by the minimum needed. Idempotent, scoped to selection if any, frames move as units and grow to contain their children
- **Right-click / long-press** context menus on nodes, edges, and the canvas
- **Search** (Cmd/Ctrl + F) — find nodes by label or description
- **Find & Replace** (Ctrl + H / Cmd + Shift + H) — bulk edit labels, descriptions, or edge labels
- **Flow validation** — flags unlabeled decisions, dead-ends, orphans, unreachable nodes. Suppress individual warnings if a structure is intentional

### Canvas
- **Pan** — hold Space + drag (or one-finger drag on touch)
- **Zoom** — hold Z + scroll, Ctrl + scroll, or pinch on a trackpad / touchscreen. 5% – 300%
- **Fit-to-content** (`⊡` or `F`) and Reset view (`⊙`)
- **Fullscreen** mode (`⛶`) hides the header + top toolbar; the floating canvas toolbar (`+ Add ▾ / ⊡ / ⊙ / ▦ / ⛶`) stays at the top-left so common actions are always one click away
- **Faint origin marker** at world (0, 0) — visible always at low opacity so Reset is predictable

### Files
- Each project can be **linked to a `.flowspec.json` file on disk** — Open, Save, Save As behave like a real document editor (Cmd/Ctrl + O / S / Shift+S)
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
Open the `⋮ More` menu → **🤖 Generate with AI…** to bring up the prompt builder. It hands you a ready-to-paste prompt that contains FlowSpec's full schema + layout rules. Drop it into any LLM (ChatGPT, Claude, Gemini, etc.), describe what you want, paste the JSON back, click Load. No file save / load step needed. Optionally include your current flow so the model can extend or refactor it.

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

## Keyboard shortcuts

| Key | Action |
|---|---|
| `1`–`8` | Add a node of that type |
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
