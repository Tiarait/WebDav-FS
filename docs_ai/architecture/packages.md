# Packages (`ua.tiar.webdavfs`)

Root: `app/src/main/java/ua/tiar/webdavfs/`

| Package / area | Role | Notable types |
|----------------|------|----------------|
| *(root)* | Application, FGS, boot | `App.kt`, `NotificationService.kt`, `BootReceiver.kt` |
| `server/` | Server runtime API | `Server.kt`, `ServerConfig.kt`, `ServerManager.kt`, `ServerController.kt`, `IServer.kt` |
| `server/fleet/` | Multi-server profiles | `ServerFleet`, `ServerProfile`, `ServerProfileStore`, `ManagedServer` |
| `server/ktor/` | WebDAV HTTP engine | `KtorServer`, engine SSL config, routes, handlers |
| `server/ftp/` | FTP engine | `FtpServer` |
| `server/ssl/` | Certificate library | `SslCertificateProfile`, `Store`, `Repository`, `Resolver`, `AutoCertificateGenerator` |
| `server/context/` | Runtime context | `ServerContext`, `AndroidServerContext`, `ServerHolder` |
| `server/data/` | FS / user models | `FileInfo`, `UserInfo`, `FsFtpFile`, … |
| `server/performance/` | Caching / IO helpers | `FileCache`, `StreamOptimizer`, … |
| `ui/` | Activity + screens | `MainActivity`, `compose/*`, `tv/*`, `home/*ViewModel` |
| `utils/` | Prefs, pro gates, net, logs | `AppSettings`, `UtilsPro`, `Logger`, `UtilsNetwork`, `UtilsPermission`, … |
| `di/` | Koin modules | `AppModule`, `ServerModule` |
| `env/` | Environment abstraction | `AppEnv`, `AndroidAppEnv` |
| `core/` | Shared types | `AppResult` |

## UI split

- **Phone:** `ui/compose/` — e.g. `HomeScreenModern`, settings lists, dialogs.
- **TV:** `ui/tv/` — `TVMainScreen`, panels for servers, users, logs, SSL.
- **Shared ViewModels:** `ui/home/` — `ServerDetailViewModel`, `ServerListViewModel`, …

Detection: `UiUtils.isRunningOnTv(context)` from `MainActivity`.
