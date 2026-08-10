# WebDavFS documentation

**WebDavFS** turns an Android phone, tablet, or TV into a **WebDAV / FTP file server** on your network so other devices can browse and transfer files.

This folder (`docs_ai/`) is written for **DeepWiki and end users**: how the product works, where to tap in the app, and what clients can expect. It is self-contained (no links outside this tree). Prefer the running app if a screen label changes.

English only.

---

## Start here

| You want… | Open |
|-----------|------|
| Setup and every setting explained | [user/guide.md](user/guide.md) |
| Short product picture | [overview.md](overview.md) |
| Metrics, logs, picker, notices | [features/app-ux.md](features/app-ux.md) |
| WebDAV extras (thumbs, sync, quota, ZIP, …) | [features/webdav-capabilities.md](features/webdav-capabilities.md) |
| Browser file manager / ZIP / Add server | [features/web-client.md](features/web-client.md) |
| HTTPS certificates (tap path) | [features/ssl-certificates-flow.md](features/ssl-certificates-flow.md) |
| FTP mode | [features/ftp.md](features/ftp.md) |
| Address, Wi‑Fi, buffer | [features/networking.md](features/networking.md) |
| Users / roles | [features/auth-users.md](features/auth-users.md) |
| Android TV | [features/tv-ui-map.md](features/tv-ui-map.md) |
| Free vs Pro / ads | [features/free-vs-pro.md](features/free-vs-pro.md) |
| Something went wrong | [troubleshooting.md](troubleshooting.md) |

---

## In the app (phone)

1. **Server list** — cards for each server; **+** to add; **gear** for app-wide settings.  
2. Open a server → tabs: **Home**, **Settings**, **Status** (metrics), **Logs**.  
3. **App settings** (gear) — theme, language, permissions, About, Feedback, Help, Ask AI.  
4. **Bell** on the list — in-app notifications inbox.  
5. Folder pickers offer **Quick Access** shortcuts (Downloads, Gallery, SD, …).  
6. First launch may show **onboarding**.

On **Android TV**, use the leanback shell (servers / settings / about) with the same features — see [features/tv-ui-map.md](features/tv-ui-map.md).

---

## Editions

| | Free | Pro |
|---|------|-----|
| Package | `ua.tiar.webdavfs` | `ua.tiar.webdavfspro` |
| Ads | Yes | No |
| Max servers | **2** | No fixed app limit |
| Max users | **2** | No fixed app limit |
| Custom headers / hidden-file tools | Limited | Unlocked |

Install both side by side if needed; use **different ports** when both servers run.

---

## Doc map

| Section | Contents |
|---------|----------|
| [user/](user/) | How-to guide |
| [overview.md](overview.md) | Product summary |
| [features/](features/) | Capabilities, settings catalog, SSL, FTP, TV, browser UI |
| [troubleshooting.md](troubleshooting.md) | Common failures |
| [architecture/](architecture/) | High-level how pieces fit (for assistants) |
| [development/](development/) | Build/release notes for maintainers |
| [build/](build/) | Flavors / SDK |
| [reference/](reference/) | Index of pages in this tree |
