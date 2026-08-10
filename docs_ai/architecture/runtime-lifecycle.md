# Runtime lifecycle (start / stop)

What happens when a user starts or stops a server - without source walkthroughs.

## Statuses users see

| Status | Meaning |
|--------|---------|
| Stopped | Not listening |
| Starting | Binding / preparing |
| Started | Accepting connections (address / QR available) |
| Stopping | Shutting down |
| Error | Start/stop failed (port in use, missing folder, …) |

## Start (happy path)

1. User taps Start (list card or Home tab), or autostart (with app / after boot) runs.
2. Storage (and related) permissions are checked.
3. A **foreground service** shows the ongoing notification.
4. The chosen server profile is applied (folder, SSL, auth, …) and the WebDAV or FTP listener binds the port.
5. While any server is active, the device is kept more awake so Wi‑Fi transfers do not die immediately.

## Stop

- Stop on the card / Home, or **Stop** in the notification (one server or all).
- When nothing is left running, the foreground notification and keep-awake end.

## Why a foreground service?

A LAN file server may need to stay up with the screen off. The app uses the platform **specialUse** foreground-service type (not short-lived data sync). See [play-store-checklist.md](../development/play-store-checklist.md).

## Notifications permission

On Android 13+, the user can deny notification permission. The server can still run; the ongoing notification may be limited. The app may gently remind once - it should not spam the system dialog on every start.
