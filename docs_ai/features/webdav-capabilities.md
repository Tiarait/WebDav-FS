# WebDAV capabilities (clients & RFCs)

What WebDavFS offers over plain “share a folder”, aimed at users and DeepWiki answers. Details of the browser UI: [web-client.md](web-client.md). Settings that affect this: [../user/guide.md](../user/guide.md).

---

## Core WebDAV (RFC 4918)

Class **1** and **2** DAV are advertised. Typical methods:

| Method | Purpose |
|--------|---------|
| `OPTIONS` | Discover support (`Dav`, allowed methods) |
| `PROPFIND` | List folders / read properties (Depth `0`, `1`, or `infinity`) |
| `PROPPATCH` | Set/remove custom (“dead”) properties |
| `GET` / `HEAD` | Download or metadata |
| `PUT` | Upload / create / overwrite files |
| `DELETE` | Remove files or folders (recursive) |
| `MKCOL` | Create a folder |
| `COPY` / `MOVE` | Copy or rename/move (with `Destination`, `Overwrite`) |
| `LOCK` / `UNLOCK` | Exclusive locks so editors do not clash |

Clients such as Windows Explorer, macOS Finder (with WebDAV), Cyberduck, Solid Explorer, and many NAS/sync apps speak this dialect.

---

## Thumbnails / image & video previews

The server can generate **listing thumbnails** for common image and video types so capable clients (including the built-in web UI) show previews without downloading the full file.

**How clients request a thumb**

- Same path as the file, with query parameters, for example:  
  `GET /path/to/photo.jpg?preview=1&x=360`
- `preview=1` (or equivalent) asks for a preview; `x=` is the max edge size in pixels (the server clamps extremes).
- Response is a small image (JPEG/PNG as appropriate), with its own cache-friendly ETag (not the same as the full file).

**Discovery**

- `OPTIONS` / `Dav` may include an `x-thumb` capability token.
- `PROPFIND` can include OwnCloud-style `oc:has-preview` when a preview is available for that item.

If a preview cannot be built, the client should fall back to the normal file icon / full GET.

---

## Byte ranges & partial uploads (HTTP)

| Feature | What it means |
|---------|----------------|
| **Range GET** | Resume downloads / stream segments → **206 Partial Content** (`Accept-Ranges: bytes`) |
| **Conditional GET** | `If-None-Match` / `If-Modified-Since` → **304** when unchanged (RFC 7232) |
| **Conditional write/delete** | `If-Match` / `If-None-Match` / `If-Unmodified-Since` → **412** if failed |
| **Partial PUT** | `Content-Range` on upload to write a byte window (resume-friendly clients) |

---

## Quota (RFC 4331)

PROPFIND can expose:

- `DAV:quota-available-bytes`
- `DAV:quota-used-bytes`

**Volume free space** for the share root is reported when useful.  
**Per-user size limits** (Users screen) need scanning the user’s files; turn on **Report quota in PROPFIND** in Advanced settings if clients need quota on every folder listing (can slow large libraries). A confirmation dialog warns about that cost.

---

## Sync collections (RFC 6578)

`REPORT` with **sync-collection** lets sync-oriented clients learn what changed since a sync token instead of re-listing everything. Mutations bump collection sync state. Advertised in `Dav` as `sync-collection`.

Useful for apps that keep a local mirror of a folder on the phone.

---

## Locks (DAV class 2)

`LOCK` / `UNLOCK` with lock tokens protect resources during editing. Missing targets return **404** (no lock-null resources). Locked resources without a valid token return **423**.

---

## ZIP / archive download

| Goal | How |
|------|-----|
| **Download a folder as ZIP** | GET the folder with `?zip=1` (or `accept=zip`), or `Accept: application/zip` — response is `FolderName.zip` |
| **Browse inside a .zip file** | In the web UI, open the archive from the item menu (peek without full extract) |

Also advertised to capable clients via DAV tokens such as `x-archive-zip`. Browser details: [web-client.md](web-client.md#download-a-folder-as-zip).

---

## OwnCloud-friendly properties

Alongside standard DAV properties, listings may include OwnCloud-namespace hints used by many mobile/desktop clients, for example:

- `oc:permissions` — compact permission string  
- `oc:has-preview` — preview available (see thumbnails above)

---

## CORS

With **CORS** enabled (default for WebDAV), browser apps on other origins can call the server (useful for web tools). Turn **CORS** off in Advanced settings if you do not need cross-origin browser access.

---

## Custom HTTP headers (Pro)

Pro builds can attach extra response headers (one `Name: value` per line) for reverse proxies, clients, or debugging. Requires a server restart when bind-critical settings change.

---

## File metadata cache

**File metadata cache** (Advanced, WebDAV, **on by default**) speeds repeated PROPFIND/HEAD. Turn it off if a client must always see disk truth immediately after external changes. No restart required; Status → Mode shows whether Cache is on.

---

## Auth & read-only

| Mode | Behavior |
|------|----------|
| Password **off** | Anonymous share of one folder; optional **Read only** |
| Password **on** | Basic auth; **Users** with per-user folders and optional quotas |

Realm name presented to clients: **WebDAV FS**.

---

## Related settings (where to tap)

Server → **Settings** → Advanced:

- Web interface / Client UI  
- CORS  
- File metadata cache  
- Report quota in PROPFIND  
- Custom headers (Pro)  
- Hidden files (Pro)

Server → **Status** → Mode chips reflect Auth / RO / Web / CORS / Cache.
