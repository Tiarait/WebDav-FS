# FTP internals

Implementation: `server/ftp/FtpServer.kt`  
Library: **Apache FtpServer** (`FtpServerFactory` / `ListenerFactory`).

Started from `Server.start()` when protocol is FTP (`userProt == "ftp"` / `serverType == "ftp"`). SSL UI does not apply to FTP.

---

## Bind

| Setting | Behavior |
|---------|----------|
| Port | `config.userPort` (must be 1–65535) |
| Host | `ipType == "all"` or empty → `0.0.0.0`; else `config.userHost` |

---

## Users & homes

- Uses default `serverFactory.userManager`.
- Each entry in `config.users` → `BaseUser(name, password)` then `userManager.save`.
- **Empty users list → throw** (config error → `FTP Server Configuration Error: …` in `Server.tryStart`).
- **Home directory** = `user.rootFolders.firstOrNull()?.second` only (first root path). Empty `rootFolders` → throw.
- Virtual FS: `FilteredFileSystemFactory` + `NativeFileSystemView` overrides (`changeWorkingDirectory`, `getHomeDirectory`, `getFile`, …), wrapping files as `FsFtpFile`.

---

## Permissions

Always attached:

- `ConcurrentLoginPermission(100, 100)`
- `TransferRatePermission(125000000, 125000000)`

If `permissions` contains `"w"` → also `WritePermission()`.

Read-only users omit write permission.

---

## Passive mode / encoding

**Not configured** in `FtpServer.kt` today:

- No custom `DataConnectionConfiguration` / passive port range.
- No explicit charset / UTF-8 listener options (library defaults).

Document this when changing FTP behavior or answering “PASV fails behind NAT” support questions.

---

## Lifecycle methods

| Method | Role |
|--------|------|
| `init()` | Validate port/host/users; build listener + users + FS factory; `createServer()` |
| `start()` | `init()` then IO coroutine `server.start()`, poll until stopped, update status |
| `stop()` | Cancel scope, `server.stop()`, clear refs |
| `isRunning` | `server?.isStopped == false` |

---

## Auth mode vs WebDAV

Same fleet `UserInfo` / `useUsers` pipeline feeds `config.users` after `applyProfile`. Anonymous / single-folder WebDAV mode still produces a usable FTP user set via defaults — verify against `Server.applyProfile` when changing auth.

Related: [auth-users.md](auth-users.md), [networking.md](networking.md).
