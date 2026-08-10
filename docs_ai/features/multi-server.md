# Multi-server

You can run **several independent servers** on one device (for example internal storage on `:8080` and an SD card folder on `:8081`).

## Rules users care about

- Each server needs its **own TCP port**.
- **Free:** up to **2** servers. **Pro:** no fixed app limit.
- Each server has its own folder (or users), SSL choice, web UI, CORS, cache, and autostart flags.
- Stopping one server does not stop the others; the notification can stop a single server or all.

## Where to go in the app

| Goal | Where |
|------|--------|
| See all servers | Server list (open the app) |
| Start / stop / address / QR | Open server → **Home** |
| Change that server | **Settings** tab |
| Live mode chips & device stats | **Status** tab — [app-ux.md](app-ux.md) |
| Session logs | **Logs** tab — [app-ux.md](app-ux.md) |
| Folder Quick Access | Folder picker shortcuts — [../user/guide.md](../user/guide.md#4-choosing-a-folder-quick-paths) |
| In-app notices | Bell on server list |
| Theme, language, About, reset everything | Gear on the **server list** |

TV uses a two-pane / D-pad layout with the same ideas — [tv-ui-map.md](tv-ui-map.md).

## Related

- Settings explained: [../user/guide.md](../user/guide.md)
- Field catalog: [settings-catalog.md](settings-catalog.md)
- Caps: [free-vs-pro.md](free-vs-pro.md)
