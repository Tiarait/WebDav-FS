# Features

## Index

| Doc | Topic |
|-----|--------|
| [multi-server.md](multi-server.md) | Profiles, ports, storage |
| [settings-catalog.md](settings-catalog.md) | UI → `ServerProfile` / `AppSettings` fields |
| [networking.md](networking.md) | Address, bind host, Wi‑Fi restart |
| [auth-users.md](auth-users.md) | Password mode, users file, free cap |
| [ssl.md](ssl.md) | Auto / Import / ACME overview |
| [ssl-certificates-flow.md](ssl-certificates-flow.md) | Cert UI → repository → Ktor apply |
| [free-vs-pro.md](free-vs-pro.md) | Caps and gated features |
| [web-client.md](web-client.md) | Bundled browser UI (deploy / routes) |
| [web-client-architecture.md](web-client-architecture.md) | JS platform + filemanager layout |
| [webdav-ktor.md](webdav-ktor.md) | Ktor/WebDAV routes & behavior |
| [webdav-errors.md](webdav-errors.md) | Error# codes → HTTP statuses |
| [ftp.md](ftp.md) | FTP bind, users, permissions |
| [tv-and-ads.md](tv-and-ads.md) | Leanback ads notes |
| [tv-ui-map.md](tv-ui-map.md) | TV navigation & screen map |

## Feature map (quick)

| Feature | Where to look |
|---------|----------------|
| Server list / start-stop | `ServerListScreen`, `HomeScreenModern`, TV `ServersPanel` |
| Per-server settings | `settingsListServerOrdered`, [settings-catalog.md](settings-catalog.md) |
| Users | `UsersManagementScreen`, `TvUsersPanel`, [auth-users.md](auth-users.md) |
| QR / share address | `DialogQr`, `shareText`, [networking.md](networking.md) |
| Logs | `LogsScreen`, `Logger`, profile `logsEnabled` |
| Certificates | `CertificateListScreen`, `SslCertificateScreen` |
| Autostart | Profile `startWithApp` / `startWithDevice`; `BootReceiver` |
| Web interface flags | Profile `useWebInterface`, `clientUi` (`modern` / `old` / `nojs`) |
