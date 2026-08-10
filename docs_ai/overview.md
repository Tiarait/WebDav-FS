# Product overview

**WebDavFS** runs a **WebDAV** (and optionally **FTP**) server on an Android device so other machines on the network can browse and transfer files.

## Editions

| | Free | Pro |
|---|------|-----|
| Package | `ua.tiar.webdavfs` | `ua.tiar.webdavfspro` |
| Ads | Yes | No |
| Max servers | **2** | No fixed app limit |
| Max users | **2** | No fixed app limit |
| Custom headers / hidden-file tools | Limited | Unlocked |

## Protocols

| Protocol | Typical URL | Notes |
|----------|-------------|--------|
| WebDAV | `http://…:port` or `https://…:port` | Default; optional browser file manager; rich client features |
| FTP | `ftp://…:port` | Per-server type; no SSL/web UI options |

WebDAV extras (thumbnails, sync, quota, locks, ZIP, ranges): [features/webdav-capabilities.md](features/webdav-capabilities.md).

## Multi-server

Each server has its own name, port, folder (or users), SSL choice, and autostart flags. Several can run at once on different ports.

See [features/multi-server.md](features/multi-server.md) and the [user guide](user/guide.md).

## Security (short)

- Prefer **password** (Users) on shared Wi‑Fi.
- Prefer **HTTPS** when exposing beyond a trusted LAN.
- **Read only** when clients should not write.
- Internet exposure (port forward / tunnel / VPN) is optional and user-managed.

## Where to read next

- End users: [user/guide.md](user/guide.md)
- Capabilities for WebDAV clients: [features/webdav-capabilities.md](features/webdav-capabilities.md)
- Browser UI / ZIP: [features/web-client.md](features/web-client.md)
- HTTPS: [features/ssl-certificates-flow.md](features/ssl-certificates-flow.md)
- FTP: [features/ftp.md](features/ftp.md)
- Free vs Pro / ads: [features/free-vs-pro.md](features/free-vs-pro.md)
- Android TV: [features/tv-ui-map.md](features/tv-ui-map.md)
