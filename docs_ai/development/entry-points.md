# Key entry points

## `App.kt`

`ua.tiar.webdavfs.App` — `MultiDexApplication`.

- Starts **Koin** (`appModule`, `serverModule`).
- Loads **`ServerFleet`**, registers Wi‑Fi / notification-related receivers as needed.
- Collects per-server **`statusFlow`** → logging, notification refresh, **WakeLock** acquire/release.
- `exitFull()` — stop fleet then kill process (used when user chooses stop-and-exit).

## `MainActivity.kt`

`ua.tiar.webdavfs.ui.MainActivity` — launcher (+ Leanback).

- Edge-to-edge Compose host.
- Phone → modern/classic Compose home; TV → `TVMainScreen`.
- Free → AdMob init.
- Hosts overlays/navigation for logs, users, SSL certificate screens.
- SAF / SD card flows via `SdCardRequestHandler`.
- Handles notification / deep-link extras to focus a server.

## `NotificationService.kt`

Foreground service that owns the “server is running” notification and start/stop commands.

- `enterForeground()` with `specialUse` / manifest FGS type.
- Dispatches to `ServerFleet` (single server, start-with-app, boot, stop-all).
- Must stay consistent with Play `specialUse` declaration.

## `BootReceiver.kt`

Receives boot / package-replaced / QuickBoot intents; starts `NotificationService` with boot extras when storage permissions allow.

## Control helpers

| Class | Role |
|-------|------|
| `ServerManager` | Start FGS intents; WakeLock API |
| `ServerFleet` | Profile CRUD + start/stop orchestration |
| `ServerDetailViewModel` / `ServerListViewModel` | UI state for one / many servers |

## DI

Koin modules in `di/AppModule.kt` and `di/ServerModule.kt`. Prefer `koinGet` / `koinViewModel` patterns already used in Compose.
