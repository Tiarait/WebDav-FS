# Permissions & SAF

## Storage access (`UtilsPermission`)

Path: `utils/UtilsPermission.kt`

| Android version | “Has storage permission” means |
|-----------------|--------------------------------|
| API ≥ 30 (R+) | `Environment.isExternalStorageManager()` (all-files access) |
| Below R | `READ_EXTERNAL_STORAGE` + `WRITE_EXTERNAL_STORAGE` |

Request path on R+: `ACTION_MANAGE_APP_ALL_FILES_ACCESS_PERMISSION` (with fallbacks to simpler settings / legacy runtime requests).

**Server start and boot** gate on `hasPermissions()` — without storage access, start flows ask for permission instead of binding the port.

---

## Notifications (API 33+)

| API | Behavior |
|-----|----------|
| `hasNotificationPermission` | `POST_NOTIFICATIONS` granted (always true below Tiramisu) |
| `consumeNotificationReminderIfNeeded` | Soft in-app dialog **once per process** when denied — does **not** block start |
| `openAppNotificationSettings` | Deep-link to app notification settings |

Do not spam the system permission dialog on every start after a permanent deny.

---

## SD card / SAF (`SdCardRequestHandler`)

Path: `utils/SdCardRequestHandler.kt`

Typical flow when the chosen folder needs tree access:

1. `setActiveServerId` for the profile being configured.
2. `promptSafGrant` / `launchSafPicker` — prefer direct SD access when possible (`tryDirectSdAccess`).
3. Else `ACTION_OPEN_DOCUMENT_TREE`.
4. `processSafResult` → `Utils.applySafTreeGrant` → patch profile `curUri` / `sdUriMap`.
5. `notifyStorageAccessUpdated` — restart server if it was running.

Failures:

- No activity to handle the tree intent → message akin to “You don't have an app that can do this” (`NO_APP_MESSAGE`) / dialogs that may open all-files settings.

Write-protection copy for users lives in string resources / help dialogs (external SD on older Android).

---

## Battery optimization

Not part of `UtilsPermission`. App settings expose battery / autostart helpers:

- `PermissionUtils.requestBatteryOptimizationExemption`
- `BatteryPermissionHelper` (OEM-specific autostart / Doze screens)
- Prefers `ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS`, then brand fallbacks

Relevant so boot / long-running server is less likely to be killed — still OEM-dependent.

---

## Where UI asks

| Place | What |
|-------|------|
| Server start buttons | Storage first; optional notification reminder after start |
| App settings | Storage + battery rows |
| Folder / SD pickers | SAF handler |
| `BootReceiver` | Skips start if storage permission missing |
