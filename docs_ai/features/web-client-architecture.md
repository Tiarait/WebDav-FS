# Browser UI notes (maintainers)

The in-app browser UI is built as a small front-end project and shipped inside the APK assets. Users only need [web-client.md](web-client.md).

## Layers (conceptually)

| Layer | Responsibility |
|-------|----------------|
| Platform | Chrome: language, dialogs, toasts, editor hooks, `window.WD` helpers |
| File manager | Listing, selection, uploads, archives, search, toolbars |

Production deploy packs a single bundle into app assets; Ktor serves it under `/~assets~/` (not a WebDAV folder path).

Do not document unpublished internal JS APIs here for DeepWiki end-user answers - stick to [webdav-capabilities.md](webdav-capabilities.md) and [web-client.md](web-client.md).
