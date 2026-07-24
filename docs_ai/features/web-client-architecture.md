# Web client JS architecture

Sources: repo `client/`. Deployed assets: `app/src/main/assets/client/`.  
Build/deploy modes: `client/README.md`. APIs: `docs/platform-api.md`, `docs/filemanager-api.md`.

---

## Bundle entry

Production (`npm run deploy`) packs:

```
client/src/entry.js
  ├── js/platform/index.js
  └── js/filemanager/index.js
```

Legacy `index.html` may queue early `window.WD.filemanager.*` calls; `entry.js` drains `_fmQueue` after load.

---

## Two layers

| Layer | Path | Role |
|-------|------|------|
| **Platform** | `src/js/platform/` | App chrome: i18n, dialogs, toast, editor, WOPI, connect, servers bridge, `window.WD` |
| **FileManager** | `src/js/filemanager/` | Listing, selection, uploads, archives, search, toolbars |

Platform `index.js` import order (simplified): i18n → core → toast → dialogs → wopi → editor → menu → **app.js** bootstrap.

FileManager public surface is re-exported from `filemanager/index.js` (`initPage`, `openListing`, selection helpers, …). Other files under `filemanager/*` are internal.

---

## Platform modules (map)

| Module | Concern |
|--------|---------|
| `core.js` / `api.js` | Shared helpers, WD namespace |
| `i18n.js` | Strings from `src/lang/*.json` (+ patches) |
| `app.js` | Bootstrap after WD ready |
| `connect.js` / `connect-panel.js` / `connect-test.js` | Standalone connect UI |
| `servers.js` / `servers-nav.js` / `servers-bridge.js` | Multi-server / fingerprint |
| `dialogs.js`, `toast.js`, `menu.js` | UX primitives |
| `editor.js`, `code-editor.js` | Text editing |
| `wopi.js` | Office integration hooks |
| `webdav-props.js`, `webdav-capabilities.js` | PROPFIND helpers |
| `dev-proxy.js` | Dev `/webdav-proxy` support |

---

## FileManager modules (map)

| Area | Folder |
|------|--------|
| Lifecycle / navigation | `core/main.js`, `core/navigation.js`, `core/state.js` |
| Listing | `listing/*` (XML PROPFIND, render, etags, handlers) |
| Selection / clipboard ops | `selection/selection.js` |
| Uploads / DnD | `core/uploads.js`, `transfer/*` |
| Archives (zip/tar) | `archive/*` |
| Search | `search/search.js` |
| UI chrome | `ui/*` (toolbars, path, user menu) |
| Settings / logs | `settings/settings.js`, `logs/logs.js` |
| Menus | `menu/*` |

---

## Server contract

Served by Ktor (see [webdav-ktor.md](../features/webdav-ktor.md)):

- Static: `/~assets~/…`
- Config: `/config-js` → flags like `canLogout`, `canViewLogs`, `webdavFsServerId`
- Data: WebDAV methods on the same origin (or external URL in standalone connect mode)

---

## Locales

- Base: `client/src/lang/{en,ru,…}.json`
- Overlays: `client/src/lang/patches/*.json`
- Sync helper: `client/scripts/sync-missing-translations.mjs`

Android app strings are separate (`app/src/main/res` + `scripts/merge_locale_strings.py`).
