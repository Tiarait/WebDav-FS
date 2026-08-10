# Packages (high level)

How the app is organized for assistants. Prefer [../user/guide.md](../user/guide.md) for “where do I tap?”.

| Area | Role |
|------|------|
| Fleet / profiles | Several servers, saved settings, start/stop |
| WebDAV engine | HTTP(S) file sharing, thumbs, sync, locks, quota, … |
| FTP engine | Optional FTP server type |
| SSL library | Shared certificate profiles (Auto / Import / ACME) |
| Phone UI | Server list, detail tabs, app settings, onboarding |
| TV UI | Leanback / D-pad shell on the same fleet |
| Announcements | Optional inbox / notices |
| Prefs & Pro gates | Theme, language, Free/Pro limits |

## Surface look (phone)

| Look | Used for |
|------|----------|
| Card surfaces | Settings rows, Status cards, server list cards |
| Elevated sheets | Connect (QR) and About |

## UI split

- **Phone** — Compose navigation: list ↔ server ↔ app settings.
- **TV** — separate leanback shell; same servers and settings ideas.

Detection is automatic on Android TV devices.
