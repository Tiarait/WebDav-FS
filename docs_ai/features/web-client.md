# Web client (browser UI)

## Role

When the WebDAV server enables the web interface, browsers can use a file-manager UI served from the same origin as the server.

Sources: repo folder **`client/`** (npm).  
Deployed into: **`app/src/main/assets/client/`**.

## Build modes

Documented in detail in `client/README.md`. Summary:

| Mode | Command | Result |
|------|---------|--------|
| Production for APK | `npm run deploy` | Bundled `app.js` + `app.css` in assets |
| Dev modules in APK | `npm run build` (+ copy) | Split ESM modules |
| Standalone dev | `npm run build && npm run serve` | Local `:5173`; WebDAV can be any URL |

## Static prefix `/~assets~/`

This is **not** a WebDAV collection path. Ktor serves UI static files under `/~assets~/…` from assets. WebDAV PROPFIND/GET for user files use other URLs (server root / configured paths).

Also relevant routes: `/config-js`, `/favicon.ico`.

## Server profile flags

- `useWebInterface` — enable/disable serving the UI
- `clientUi` — e.g. `modern` / `old` / `nojs`
- Logs may optionally surface in the web client (`logsShowInWebClient`)

## JS APIs

Long-form API references may exist in the full source repository as `docs/platform-api.md` (`window.WD`) and `docs/filemanager-api.md`. Module layout in this tree: [web-client-architecture.md](web-client-architecture.md).
