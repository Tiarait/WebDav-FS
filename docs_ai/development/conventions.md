# Conventions

## Documentation (`docs_ai/`)

- Written for **DeepWiki and end users**: how to use and configure the app, what clients can do.
- **Self-contained** - no required links outside this tree.
- Avoid leaking implementation secrets (internal paths, handler class names, unpublished APIs). Capability docs are fine (RFCs, thumbs query shape, HTTP statuses).
- When the app and docs disagree, **trust the app**, then update docs.

## Product changes (maintainers)

- Match existing style; minimal diffs.
- Keep Free / Pro boundaries.
- Do not commit secrets.
- Phone and TV UX both matter for start/share/permissions.
- Strings: EN first, then locale merge - [localization.md](localization.md). Prefer ASCII `-` in user-facing copy.
