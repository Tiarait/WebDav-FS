# Dependency injection (Koin)

Started in `App.kt`: `modules(appModule, serverModule)`.

There are **no** `factory { }` blocks — only `single` and `viewModel`.

---

## `di/AppModule.kt`

| Binding | Kind |
|---------|------|
| `AppEnv` ← `AndroidAppEnv` | single |
| `AppSettings` | single |
| `LockManager` | single |
| `ServerProfileStore` | single |
| `SslCertificateStore` | single |
| `SslCertificateRepository` | single |
| `ServerFleet` | single |
| `ServerHolder` | single |
| `ServerManager` | single |
| `App` (from `Context`) | single |
| `CoroutineScope` (app) | single |
| `MutableState<Boolean>` (`visibleUi`) | single |
| `() -> Unit` (`exitFull`) | single |
| `ServerController` ← `ServerControllerImpl` | single |
| `ServerUiViewModel` | viewModel |
| `ServerListViewModel` | viewModel |
| `ServerDetailViewModel(serverId)` | viewModel (parameters) |

---

## `di/ServerModule.kt`

All `single`:

- `WebDavRenderer` ← `WebDavRendererImpl`
- `ServerContext` ← `AndroidServerContext`
- Handlers: `PropFindHandler`, `PropPathHandler`, `HeadHandler`, `GetHandler`, `PutHandler`, `DeleteHandler`, `MkdirHandler`, `MoveHandler`, `CopyHandler`, `LockHandler`

Ktor installs these handlers from the DI graph when serving WebDAV.

---

## Usage in UI

Compose/TV typically use `koinViewModel` / `koinGet` (see existing screens). Prefer injecting `ServerFleet` / repositories rather than constructing stores manually.
