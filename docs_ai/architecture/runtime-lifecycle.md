# Runtime lifecycle (start / stop / FGS / WakeLock)

## Status machine

`Server.Status` (`server/Server.kt`):

| Status | Meaning |
|--------|---------|
| `Stopped` | Not running |
| `Starting` | Binding / configuring |
| `Started` | Accepting connections |
| `Stopping` | Shutting down |
| `Error(message)` | Failed start/stop |

UI and `App` observe per-server `statusFlow` on `ManagedServer` / runtime.

## Start path (happy path)

1. User taps start (Compose / TV) or boot / “start with app”.
2. Storage permissions checked (`UtilsPermission.hasPermissions`).
3. `ServerManager` starts **`NotificationService`** as a foreground service with extras (`serverId`, boot flags, …).
4. Service calls `enterForeground()` then on a worker thread:
   - Boot → `ServerFleet.startOnBoot()` (`startWithDevice` profiles)
   - Start-with-app → `ServerFleet.startWithApp()` (`startWithApp` profiles)
   - Else → `fleet.startServer(serverId)` (or default)
5. `ServerFleet.startServer` → ensure runtime → `applyProfile` (+ resolve SSL cert) → `Server.start()` → `KtorServer` or `FtpServer`.
6. On `Started`, `App` / `ServerManager` **acquires WakeLock** and refreshes the notification (address, stop actions).

## Stop path

- UI stop, notification action, or `exitFull()` / stop-all.
- Commands into `NotificationService` (`CMD_STOP`, `CMD_STOP_ALL`, …).
- `fleet.stopServer` / `stopAll`.
- When `!fleet.anyActive()` → **release WakeLock** and tear down foreground if appropriate.

`anyActive()` treats Started / Starting / Stopping as active so locks are not dropped mid-restart (e.g. Wi‑Fi change).

## Foreground service (`specialUse`)

| Item | Detail |
|------|--------|
| Service | `NotificationService` |
| Type | `foregroundServiceType="specialUse"` |
| Permission | `FOREGROUND_SERVICE_SPECIAL_USE` |
| Property | `PROPERTY_SPECIAL_USE_FGS_SUBTYPE` (Play declaration text) |
| Why not `dataSync` | Platform time limits (~6h) are unsuitable for a user-started LAN server |

Play Console rationale and subtype wording: [play-store-checklist.md](../development/play-store-checklist.md). A longer draft may exist as `docs/PLAY_FGS_SPECIAL_USE.md` in the full source repository.

Runtime enter: `ServiceCompat.startForeground` with `FOREGROUND_SERVICE_TYPE_MANIFEST`, fallback `SPECIAL_USE` on API 34+.

**Notifications:** On Android 13+, `POST_NOTIFICATIONS` may be denied. Server can still run; UI may show a soft once-per-process reminder to open system settings (does not block start).

## WakeLock (two layers)

| Layer | Owner | Role |
|-------|-------|------|
| Short service lock | `NotificationService` | While handling start/stop work in the service |
| Server lock | `ServerManager` (`webdavfs::ServerWakeLock`) | `PARTIAL_WAKE_LOCK` while any server is active |

Release server lock only when the fleet is idle.

## Boot

`BootReceiver` listens for `BOOT_COMPLETED`, `LOCKED_BOOT_COMPLETED`, `MY_PACKAGE_REPLACED`, QuickBoot variants, etc. If permissions OK, starts `NotificationService` with `EXTRA_FROM_BOOT`.

## Important classes

- `App.kt` — Koin, fleet status collection, WakeLock side effects, `exitFull()`
- `NotificationService.kt` — FGS + command handling
- `ServerManager.kt` — intents into the service + WakeLock API
- `ServerFleet.kt` — profile CRUD and orchestration
- `BootReceiver.kt` — cold start / update
