# Notifications & intent extras

Service: `NotificationService.kt`  
Starter API: `server/ServerManager.kt`  
Boot / action relay: `BootReceiver.kt`

FGS type and lifecycle: [../architecture/runtime-lifecycle.md](../architecture/runtime-lifecycle.md).

---

## Action / extras / commands

| Kind | Name | Notes |
|------|------|--------|
| Action | `ACTION` | `ua.tiar.webdavfs.NOTIFICATION_BUTTON` or `…webdavfspro…` by flavor |
| Extra | `EXTRA_CMD` | `"cmd"` |
| Extra | `EXTRA_SERVER_ID` | `"server_id"` |
| Extra | `EXTRA_OPEN_SERVER_ID` | `"open_server_id"` (open UI) |
| Extra | `EXTRA_FROM_BOOT` | `"from_boot"` |
| Extra | `EXTRA_START_ENABLED` | `"start_enabled"` (start-with-app) |
| Cmd | `CMD_STOP` | `"stop"` |
| Cmd | `CMD_STOP_ALL` | `"stop_all"` |
| Cmd | `CMD_REFRESH` | `"refresh"` |
| Legacy | extra `"id" == "stop"` | Treated as stop-all |

---

## `onStartCommand` routing

1. `CMD_STOP` + server id → stop one.
2. `CMD_STOP_ALL` (or legacy stop) → stop all + teardown when idle.
3. `CMD_REFRESH` → sync notifications or tear down if nothing running.
4. Else → `enterForeground()`, background work:
   - `EXTRA_FROM_BOOT` → `fleet.startOnBoot()`
   - `EXTRA_START_ENABLED` → `fleet.startWithApp()`
   - else `EXTRA_SERVER_ID` → `startServer`, else default server

Also refreshes hosts before start when appropriate.

---

## `ServerManager` builders

| Method | Intent payload |
|--------|----------------|
| `startFleetService(serverId?, fromBoot?, startEnabled?)` | optional server id / boot / start-enabled → start FGS |
| `refreshNotification()` | `CMD_REFRESH` |
| `stopServerInFleet(id)` | `CMD_STOP` + id |
| `stopAllInFleet()` | `CMD_STOP_ALL` |

WakeLock acquire/release is owned here in coordination with fleet `anyActive()`.

---

## Notification PendingIntents

Built in `buildServerNotification` (per active server as implemented):

| Control | Target | Payload |
|---------|--------|---------|
| Content tap | `MainActivity` | `EXTRA_OPEN_SERVER_ID` |
| Stop action | `BootReceiver` broadcast (`ACTION`) | `CMD_STOP` + `EXTRA_SERVER_ID` |

`BootReceiver` forwards the notification action into the service with the same extras.

Free and Pro use **different** action package names / applicationIds so they do not collide when both are installed.
