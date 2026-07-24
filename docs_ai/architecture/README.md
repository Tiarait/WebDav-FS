# Architecture

## Index

| Doc | Topic |
|-----|--------|
| [modules.md](modules.md) | Gradle modules and `client/` |
| [packages.md](packages.md) | Java/Kotlin packages under `app` |
| [runtime-lifecycle.md](runtime-lifecycle.md) | Start/stop, FGS `specialUse`, WakeLock |
| [diagrams.md](diagrams.md) | Mermaid: start/stop, Wi‑Fi, ACME |

## Mental model

```
UI (Compose / TV)
    │  start / stop / edit profile
    ▼
ServerFleet  ──► ServerProfileStore (JSON on disk)
    │
    ▼
Server (per profile runtime)
    ├── KtorServer   (WebDAV)
    └── FtpServer    (FTP)
    │
    ▼
NotificationService (foreground) + ServerManager WakeLock
```

**SSL** is a separate library of certificate profiles (`SslCertificateRepository`). A server profile only stores `useSsl` + `sslCertificateId`.

**AppSettings** holds app-wide prefs (theme, language, legacy migration, ad counters), not the full per-server config (that lives in fleet profiles).
