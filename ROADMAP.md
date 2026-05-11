# FlowSpec roadmap

Things on the runway, no committed dates. Order is rough priority, not schedule.

## Likely next

- **User-created templates** — let users save the current project as a template that shows up in the `+ From template…` gallery alongside the built-ins. Editable / deletable from the same gallery view. Stored in localStorage initially.
- **PWA install** — `manifest.json` + a tiny service worker. Becomes installable to home screen / desktop, runs offline as an app.
- **Share via URL** — encode the entire project into a URL hash so someone can open a flow you sent them with no file involved. Works for small/medium flows (~50 nodes max before URL length becomes a real issue).

## Editor polish

- **Animated edges** — subtle flowing-dots animation along edges. Off by default, opt-in per project.
- **Custom output formats** — YAML, DOT/Graphviz, prompt-template wrapping ("Implement this:" / "Review this:" boilerplate around the spec).
- **Edge waypoints quality of life** — auto-route around nodes when both endpoints exist, double-click on the line to add a waypoint at that point.

## Bring-your-own-key LLM features

These would call an LLM provider directly using a key the user pastes into Settings. Stored client-side, never sent to any FlowSpec server (there isn't one).

- **Generate a flow from a description** — same idea as the existing Generate-with-AI modal but skipping the copy/paste round-trip.
- **Refine via LLM** — "Extend this with error handling," "Add a retry path," etc. Operates on the current project and applies the diff.

## Future / maybe

- **Mini-map** — small overview in the corner for navigation in big flows.
- **Cloud sync** — optional, opt-in per project. GitHub Gists or similar so projects survive browser data loss without becoming a server-shaped product.
- **Comments on nodes / edges** — separate from `description`, side-conversation that doesn't show in the spec output by default.
