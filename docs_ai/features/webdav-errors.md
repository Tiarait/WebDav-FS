# WebDAV / HTTP statuses clients see

When something fails, clients should rely on the **HTTP status**. The app may also log a short internal label; users rarely need those.

| HTTP | Typical meaning |
|------|-----------------|
| **201** | Created (new upload, new folder, copy/move to new path) |
| **204** | Success with no body (update, delete, overwrite) |
| **206** | Partial content (byte-range download) |
| **207** | Multi-Status (folder listing / property results) |
| **304** | Not modified (caching / ETag) |
| **400** | Bad request (malformed headers or lock body) |
| **401** | Need login (Basic auth) |
| **403** | Forbidden (storage / access failure) |
| **404** | Missing path |
| **405** | Not allowed (no permission, or path hidden by settings) |
| **409** | Conflict (already exists, bad parent, etc.) |
| **412** | Precondition failed (Overwrite, If-*, Destination) |
| **413** | Too large / not enough free space |
| **416** | Range not satisfiable |
| **423** | Locked (need a valid lock token) |
| **500** | Unexpected server error |
| **507** | Insufficient storage / user quota exceeded |

**Hidden files** (Pro): filtered paths often look like “method not allowed” to clients even though the file exists on disk.

**Quota:** uploads over a user’s limit return **507**.

More capabilities: [webdav-capabilities.md](webdav-capabilities.md).
