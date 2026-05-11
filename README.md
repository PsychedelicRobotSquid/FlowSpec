# FlowSpec

A visual flowchart builder that outputs LLM-friendly structured text.

Drag-and-drop boxes onto a canvas, connect them, label them — and as you build, the side panel streams out a clean structured spec you can paste straight into an AI prompt. Designed for the gap between "tools you draw in" (Excalidraw, draw.io, Whimsical) and "tools that output text an LLM can understand."

> No installation. No build step. No dependencies. One HTML file, opened in any browser.

## Why this exists

Most visual diagramming tools export images or proprietary formats. When you want to describe a flow to an LLM, you end up either:

1. Writing it out by hand as prose (slow, error-prone)
2. Generating Mermaid syntax (text-first, not visual)
3. Exporting an image and uploading it (works, but the LLM has to re-derive structure)

FlowSpec inverts the workflow: **lay it out visually, get clean text out.** The output is structured Markdown by default — node list, connections list, plus an ASCII tree of the flow — exactly what an LLM needs to understand structure without ambiguity.

## Features

- **Visual canvas** with drag-to-move nodes, connect-with-clicks, and grid snapping
- **8 node types** — Start, Process, Decision, End, Folder, File/Data, Screen, Note — each with distinct shapes and colors
- **Multi-select** via marquee drag or shift-click, with align and distribute tools
- **Pan & zoom-style navigation** — hold Space to pan, or toggle the Pan tool
- **Undo / redo** — 50 steps, keyboard shortcuts
- **Live text output** in 5 formats — Structured Markdown, Narrative, Folder tree (ASCII), JSON, Mermaid
- **Multi-project workspace** — keep several flows, switch between them, all auto-saved to your browser
- **Import / export** as JSON for backup, sharing, or version control
- **Zero dependencies** — pure HTML, CSS, and vanilla JavaScript. Works offline.

## Quick start

1. Download `flowspec.html`
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. Build your flow
4. Copy the text output and paste it into your AI prompt

That's it. Your work auto-saves to `localStorage`, so you can close the tab and come back.

## Output formats

| Format | What it's good for |
|---|---|
| **Structured Markdown** | The default. Best for pasting into Claude/ChatGPT as a spec. Lists nodes by type, all connections enumerated, plus an ASCII tree. |
| **Narrative** | Prose per node ("Authenticate: POST /auth, store token. Leads to: Show dashboard."). Good when intent matters more than structure. |
| **Folder tree (ASCII)** | `tree`-style folder layout. Drop in Folder and File/Data nodes, connect parents → children, and get clean directory structure. |
| **JSON** | Machine-readable. Feed into scripts, version-control your specs, or have an LLM generate code from the graph. |
| **Mermaid** | The standard Mermaid `flowchart TD` syntax. Paste into GitHub READMEs, Notion, or any Mermaid renderer to display the diagram. |

## Keyboard shortcuts

| Key | Action |
|---|---|
| `C` | Toggle Connect mode |
| `Space` (hold) + drag | Pan canvas |
| `Shift` + click | Add/remove node from selection |
| `Cmd/Ctrl` + `A` | Select all nodes |
| `Cmd/Ctrl` + `Z` | Undo |
| `Cmd/Ctrl` + `Shift` + `Z` (or `Ctrl` + `Y`) | Redo |
| `Delete` / `Backspace` | Delete selection |
| `Esc` | Cancel current mode / clear selection / close dialog |

## Example workflow

You want Claude to scaffold an Expo TypeScript app. Instead of writing out the file structure in prose:

1. Open FlowSpec
2. Drop in a few Folder nodes: `src/`, `screens/`, `components/`
3. Drop in File/Data nodes for files: `App.tsx`, `Button.tsx`, etc.
4. Connect parents to children
5. Switch the output format to **Folder tree (ASCII)**
6. Copy and paste into your prompt:

```
project-root/
├── src/
│   ├── components/
│   │   ├── Button.tsx
│   │   └── Modal.tsx
│   └── App.tsx
└── README.md
```

For app/program flows, use Start, Decision, Process, Screen, and End nodes, then copy the **Structured Markdown** output.

## Tech notes

- Single HTML file (`flowspec.html`), ~2000 lines including CSS and JS
- No build, no bundler, no dependencies
- SVG-based canvas; positions stored as world coordinates
- Auto-saves to `localStorage` under the key `flowspec.workspace.v1`
- Migration is automatic if you used an older single-project version

## Roadmap ideas

Things that aren't built but might be:

- Auto-layout (right now node positions are fully manual)
- Zoom controls (the existing pan handles navigation but doesn't scale)
- Image export (PNG/SVG of the canvas itself)
- Edge routing improvements (curves for overlapping connections)
- Templates library (common flow patterns as one-click starters)

PRs welcome.

## License

MIT — do whatever you want with it.

## Credits

Built with [Claude](https://claude.ai) in a few back-and-forth iterations.
