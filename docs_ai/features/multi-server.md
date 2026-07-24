# Multi-server (fleet)

## Concepts

| Type | Role |
|------|------|
| `ServerProfile` | Persistent JSON config (port, folder, SSL id, users flags, autostart, …) |
| `ServerProfileStore` | Load/save profiles on disk |
| `ManagedServer` | Profile + optional live `Server` runtime |
| `ServerFleet` | In-memory list; start/stop/patch/save; free-tier limits |

Legacy single-server prefs in `AppSettings` migrate once into the default profile on first fleet load.

## Rules

- **Port uniqueness:** two profiles must not share a bind port; start fails if the port is taken.
- **Free cap:** `ServerFleet.FREE_MAX_SERVERS` (**2**). Pro: no fixed cap in app code.
- **Protocols:** per profile `serverType` = `webdav` | `ftp`; `useSsl` applies to WebDAV only.

## Status

Each runtime exposes `statusFlow` (`Stopped` → `Starting` → `Started`, etc.). The notification and WakeLock track whether **any** server is active (`anyActive()`).

## Data on disk (typical)

- Profile store (fleet JSON) — see `ServerProfileStore` implementation for exact path.
- Per-server data under app files (e.g. users JSON under `servers_data/{id}/`).
- Certificates under the global SSL store (`filesDir/certificates/…`) — not embedded in each profile beyond `sslCertificateId`.

## UI

- Phone: server list + detail (`HomeScreen` / `HomeScreenModern`) + sheet settings.
- TV: `ServersPanel` + detail ViewModels keyed by server id.

Deeper field reference for developers: [settings-catalog.md](settings-catalog.md). End-user explanations: [../user/guide.md](../user/guide.md).
