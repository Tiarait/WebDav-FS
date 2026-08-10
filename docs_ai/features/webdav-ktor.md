# WebDAV server behavior

User-facing capability list (thumbs, RFCs, quota, sync, locks, ZIP): **[webdav-capabilities.md](webdav-capabilities.md)**.

This page is a short map for assistants answering “what does the server do?” without diving into source layout.

## Protocols & UI

- **WebDAV** over HTTP or HTTPS (per-server SSL).
- Optional **browser file manager** when Web interface is on ([web-client.md](web-client.md)).
- **FTP** is a separate server type (no SSL/web UI options) — [ftp.md](ftp.md).

## Client-visible extras

| Area | Summary |
|------|---------|
| Listing thumbs | `GET …?preview=1&x=…`; `oc:has-preview` in PROPFIND |
| Ranges | Partial download / conditional requests |
| Partial upload | `Content-Range` PUT when the client supports it |
| Quota | RFC 4331 properties; optional deep listing (setting) |
| Sync | RFC 6578 sync-collection REPORT |
| Locks | LOCK / UNLOCK (DAV class 2) |
| Archive | ZIP download of folders |
| CORS / cache / headers | Tunable in Advanced settings |

## Errors clients see

Prefer **HTTP status codes**. A short map: [webdav-errors.md](webdav-errors.md).

## Related

- Configure in the app: [../user/guide.md](../user/guide.md)
- Field names for settings: [settings-catalog.md](settings-catalog.md)
