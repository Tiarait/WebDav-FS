# WebDavFS documentation

Public documentation for **WebDavFS** — an Android app that turns your phone, tablet, or TV into a **WebDAV / FTP file server** on your network.

This folder is meant to be published (GitHub, DeepWiki, static site, etc.). It serves:

- **End users** — how to run and configure the app  
- **Developers & AI assistants** — architecture, internals, and contribution-oriented notes  

English only. Prefer **source code** if something here drifts from the app.

---

## Publishing this folder

You can publish **`docs_ai/` alone** (GitHub Pages, DeepWiki, a docs site). Point the index at `README.md`.

- Self-contained for users: [user/guide.md](user/guide.md)  
- Self-contained for most technical Q&A: `architecture/`, `features/`, `development/`, `troubleshooting.md`  
- Mentions of a sibling `docs/` folder refer to **optional** deeper essays in the full source repository. They are **not** required for this tree to make sense.

---

## For users

| Doc | Contents |
|-----|----------|
| [user/guide.md](user/guide.md) | Full how-to: quick start, every setting, connect, troubleshooting |
| [overview.md](overview.md) | Short product overview (Free vs Pro, protocols) |
| [troubleshooting.md](troubleshooting.md) | Common failures (user + technical) |

Start here if you just installed the app: **[User guide](user/guide.md)**.

---

## For developers & AI assistants

| Section | Contents |
|---------|----------|
| [architecture/](architecture/) | Modules, packages, lifecycle, diagrams |
| [features/](features/) | Settings catalog, networking, auth, SSL, WebDAV/FTP, TV, web client |
| [build/](build/) | Flavors, SDK, signing, locales |
| [development/](development/) | Entry points, DI, permissions, Play checklist |
| [reference/](reference/) | Optional deeper files in the full source repo |

Hard facts (from application code):

| Fact | Value |
|------|--------|
| Free max servers | 2 (`ServerFleet.FREE_MAX_SERVERS`) |
| Free max users | 2 (`UtilsPro.FREE_MAX_USERS`) |
| Free package | `ua.tiar.webdavfs` |
| Pro package | `ua.tiar.webdavfspro` |
| `targetSdk` / `compileSdk` | 36 |
| Foreground service type | `specialUse` (not `dataSync`) |

---

## Repository layout

```
WebDavFS/
  app/       Android application
  free/      Free flavor (ads)
  pro/       Pro flavor
  acme/      Let's Encrypt helpers
  client/    Browser file-manager UI (npm)
  docs/      Additional long-form notes (SSL design, Play FGS text, JS APIs)
  docs_ai/   This documentation tree (publishable)
  scripts/   Locale merge helpers
```

---

## Editions

| Edition | Application id | Notes |
|---------|----------------|--------|
| Free | `ua.tiar.webdavfs` | Ads; limited servers/users; some Pro-only options |
| Pro | `ua.tiar.webdavfspro` | No ads; Pro features unlocked |

Both can be installed side by side (use different ports if both run servers).
