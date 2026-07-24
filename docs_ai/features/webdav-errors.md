# WebDAV Error# codes & HTTP statuses

Internal codes appear in logs and sometimes in JSON bodies (`{"error":"Error#N","message":"…"}`).  
HTTP status is what clients should rely on. Prefer updating this table when handlers change.

Related: [webdav-ktor.md](webdav-ktor.md).

---

## Catalog

| Code | Typical HTTP | Meaning | Where |
|------|--------------|---------|--------|
| `Error#0` | *(start failure)* | Cannot generate IP / empty host | `Server.kt` logs |
| `Error#2` | *(start failure)* | Exception while starting (logged; may still surface as `Status.Error`) | `Server.kt` |
| `Error#9` | **405** | Permission denied (read or write gate) | `RouteWebdav` `checkCanRead` / `checkCanWrite` |
| `Error#15` | **405** | Path is hidden (hide-hidden / hidden list) | PROPFIND, GET, PROPPATCH, PUT/DELETE/MOVE/COPY/MKCOL |
| `Error#400` | **400** | Bad LOCK request (e.g. missing token bits) | `LockHandler` |
| `Error#403` | **403** | Access / FS / SAF failure mapped as forbidden | Mutating handlers |
| `Error#404` | **404** | Missing resource | Most handlers; PROPFIND also plain 404 |
| `Error#405` | **405** | Method not allowed (e.g. MKCOL conflict variant) | `MkdirHandler` |
| `Error#409` | **409** | Conflict (exists, collection PUT, move into self, …) | MKCOL, PUT, MOVE, COPY |
| `Error#412` | **412** | Precondition failed (Overwrite:F, missing Destination, If-*, …) | MOVE, COPY, PUT, LOCK |
| `Error#423` | **423** | Locked without valid token | PUT, DELETE, LOCK, MOVE/COPY dest |
| `Error#500` | **500** | Unexpected / IO failure | Catch-alls |
| `Error#NOSPACE` | **507** | Quota / insufficient storage | `PutHandler` |
| `Error#TOOLARGE` | **413** | Payload too large / disk reserve | `PutHandler` |

---

## Status without Error# label

| HTTP | Situation |
|------|-----------|
| **201** | Created (PUT new, MKCOL, MOVE/COPY to new dest) |
| **204** | No content (PUT update, DELETE, overwrite MOVE/COPY, OPTIONS proxy) |
| **206** | Partial content (Range GET) |
| **207** | Multi-Status (PROPFIND XML; PROPPATCH ack) |
| **304** | Not modified (ETag / If-Modified-Since) |
| **401** | Unauthorized (Basic challenge) |
| **416** | Range not satisfiable |
| **502** | Upstream proxy failure (`/webdav-proxy`) |
| **413** | Explicit “File too large” text from PUT |
| **507** | Explicit quota exceeded text from PUT |

---

## Permission vs hidden

- **Error#9** — user/role cannot read or write (authz).
- **Error#15** — resource filtered as hidden by server settings (still may exist on disk).

Both often return **405** with a small JSON body for API-ish clients; traditional WebDAV clients should treat the status code first.
