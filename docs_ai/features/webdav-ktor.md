# WebDAV (Ktor) internals

Package root: `app/src/main/java/ua/tiar/webdavfs/server/ktor/`

## Engine lifecycle

`KtorServer.kt`:

1. `start()` → `init()` builds Netty `embeddedServer` via ktor `ServerConfig(config, sslCertificateId)`.
2. Installs `module()`; stores owning `Server` on application attributes (`KtorServerOwnership` → handlers can reach config, `LockManager`, `FileCache`).
3. `server.start(wait = false)`; status mirrors Netty lifecycle (`Starting` / `Started` / …).

### Plugins in `module()`

| Plugin | Notes |
|--------|--------|
| `CallLogging` | Skips noisy paths: `/~assets~/`, `/favicon.ico`, `/config-js`, `/webdav-proxy` |
| `DefaultHeaders` | `Server: WebDAV FS/<version>`, `Dav: 1, 2`, optional `X-WebDAV-FS-Server`, CORS, custom headers |
| `Compression` | gzip |
| `Authentication` | Basic, realm `WebDavAuth.REALM` (`"WebDAV FS"`) |

---

## Authentication

`WebDavAuth.kt` + validate in `KtorServer`:

- Lookup `config.findUser(name)`; password empty or match → `UserIdPrincipal`.
- `passEnabled` + unknown user → reject.
- Auth off + unknown → guest principal `""`.
- When `passEnabled`, WebDAV routes sit under `authenticate(REALM)`; unauthenticated → 401 + `WWW-Authenticate` (minimal body).
- Auth off: routes open; Basic may still be parsed for per-user roots (`RouteWebdav.parseBasicAuth`).

Write methods: `checkCanWrite` (deny → 405 JSON `Error#9`).  
Read (PROPFIND/GET/HEAD): `checkCanRead`.  
PROPPATCH: **no** read/write gate.

---

## Top-level routes (`KtorServer.kt`)

| Path | Methods | Notes |
|------|---------|--------|
| `/.well-known/acme-challenge/{token}` | GET | `RouteAcmeChallenge` (ACME HTTP-01) |
| `/favicon.ico` | GET | If `useWebInterface` |
| `/check_space` | HEAD | `X-Free-Bytes` from user root |
| `/~assets~/{path...}` | GET | Static UI if JS client enabled; rejects `..` |
| `/config-js` | GET | Runtime JS config; Basic if `passEnabled` |
| `/logs` | GET | Admin logs if auth + profile flags |
| `/webdav-proxy`… | any | `RouteWebdavRemoteProxy` (JS client) |
| WebDAV catch-all | see below | `RouteWebdav` (± authenticate) |

---

## WebDAV method table (`routes/RouteWebdav.kt`)

Mounted on `/{resourceId...}`, trailing slash, `/`, `""`. Also `GET /logout` (forces next GET 401).

| Method | Perm | Handler | File |
|--------|------|---------|------|
| OPTIONS | — | inline | `RouteWebdav.kt` |
| PROPFIND | read | `PropFindHandler` | `handlers/PropFindHandler.kt` |
| PROPPATCH | — | `PropPathHandler` | ack-only 207 |
| GET | read | `GetHandler` | |
| HEAD | read | `HeadHandler` | |
| PUT | write | `PutHandler` | |
| DELETE | write | `DeleteHandler` | |
| MKCOL | write | `MkdirHandler` | |
| MOVE | write | `MoveHandler` | |
| COPY | write | `CopyHandler` | |
| LOCK | write | `LockHandler` | in-memory `LockManager` |
| UNLOCK | write | `LockHandler` | |

OPTIONS advertises: `OPTIONS, GET, HEAD, PUT, DELETE, PROPFIND, PROPPATCH, MKCOL, COPY, MOVE, LOCK, UNLOCK` + `Dav: 1, 2`, `MS-Author-Via: DAV`, `Accept-Ranges: bytes`.

Supporting: `WebDavResourceResolver` (FS + SAF), `renderer/WebDavRenderer` (PROPFIND XML / HTML listing).

---

## Method behavior highlights

### PROPFIND

- **Depth** header default `"1"`; supports `0` / `1` / `infinity`.
- Listings via `FileSystemUtils.listFiles`; nested trees for deep depth.
- **Quota** on dirs when `reportQuotaInPropfind` + limited user → `DAV:quota-available-bytes` / `quota-used-bytes`.
- ETag files `"mtime-size"`; dirs include child stats. Conditional → 304.
- Hidden → 405 `Error#15`; missing → 404; success → **207** XML.
- **FileCache** for metadata/listings when enabled.

### PUT

- Quota pre-check → **507** if cannot upload.
- Disk reserve (~100MB free after write) else **413**; unknown length capped ~2GB.
- Locks without token → **423**; conditionals → **412**.
- Overwrite = delete+create; cannot PUT onto collection → **409**.
- **201** / **204**; invalidates cache; body via `StreamOptimizer.copyStream`.

### DELETE

- **No Depth** — always recursive.
- Lock / If-Match → 423 / 412; success **204**.

### MKCOL

- Exists → **409**; **201** + `Location`; FS `mkdirs` or SAF `createDocument`.

### MOVE / COPY

- Require `Destination` (missing → **412**).
- **Overwrite** default `T`; `F` + exists → **412**.
- Locks → **423**. MOVE into self → **409**. COPY same path → **409**.
- **Depth not read** — always full tree.
- **201** (new) / **204** (overwrite).

### LOCK / UNLOCK

- In-memory exclusive write locks; `Timeout: Second-N`.

---

## CORS & custom headers

From profile via `Server.applyProfile` → engine `ServerConfig`:

- CORS on unless `corsMode == "off"`: `Allow-Origin: *`, credentials, WebDAV-oriented Allow-Headers/Methods, Expose-Headers including `Content-Range`, ETag, `X-WebDAV-FS-Server`.
- Custom headers: **Pro only**, parsed from `customHeadersRaw` (`Name: value` lines) into `DefaultHeaders`.

---

## Range, cache, streams

| Piece | Role |
|-------|------|
| `WebDavHeaders.parseRangeHeader` | `bytes=…` → **206** / **416** |
| `FileCache` | PROPFIND listings/metadata; invalidated on mutating methods; cleared on `Server.start()` |
| `StreamOptimizer` | PUT copy; GET range skip+copy; buffer from profile `effectiveBufferSize` |

---

## SSL entry (Ktor)

| Step | Where |
|------|--------|
| Profile → config | `Server.applyProfile` + `SslCertificateResolver` |
| Connectors | `ktor/ServerConfig.kt` `configuration` / `configurationSSL` |
| Keystore | `resolveKeyStore()`: custom (`CertificateLoader`), acme (`AcmeCertificateStorage`), else auto (`AutoCertificateGenerator`) |
| ACME HTTP-01 | `RouteAcmeChallenge` |

Certificate product flow: [ssl-certificates-flow.md](ssl-certificates-flow.md).

---

## Package map

```
ktor/
  KtorServer.kt
  ServerConfig.kt
  WebDavAuth.kt
  WebDavHeaders.kt
  KtorServerOwnership.kt
  routes/          RouteWebdav, RouteWebdavRemoteProxy, RouteAcmeChallenge
  handlers/        one class per method family
  renderer/        WebDavRenderer
```
