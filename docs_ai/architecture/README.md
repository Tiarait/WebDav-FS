# Architecture

High-level picture for DeepWiki. For setup questions, use [../user/guide.md](../user/guide.md) and [../features/webdav-capabilities.md](../features/webdav-capabilities.md).

## Index

| Doc | Topic |
|-----|--------|
| [modules.md](modules.md) | App flavors and related projects |
| [packages.md](packages.md) | Major areas of the app |
| [runtime-lifecycle.md](runtime-lifecycle.md) | Start/stop, foreground service, wake lock |
| [diagrams.md](diagrams.md) | Mermaid overviews |

## Mental model

```
UI (phone or TV)
    │  start / stop / edit settings
    ▼
Server fleet (several profiles)
    │
    ▼
Per-server runtime
    ├── WebDAV (HTTP/HTTPS)
    └── FTP (optional type)
    │
    ▼
Foreground notification + keep-awake while any server is active
```

**SSL certificates** live in a shared library; each server only selects one profile.  
**App settings** (theme, language, permissions, About) are global; per-server options live on each server’s Settings tab.
