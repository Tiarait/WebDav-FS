# TV UI map

Entry: `MainActivity` → `UiUtils.isRunningOnTv` → **`TVMainScreen`**.

Navigation types: `ui/tv/TvNavigation.kt`, sub-screens: `TvServerSubScreen.kt`.

---

## Shell

| Piece | File | Role |
|-------|------|------|
| Root | `TVMainScreen.kt` | Hosts panels, permissions, QR, sub-screen state |
| Layout | `SlidingPanelContainer.kt` | Sidebar + content; focus levels |
| Nav rail | `NavigationPanel.kt` | Servers / Settings / About / Exit |
| Header | `TVHeader.kt` | Branding / actions |
| Theme | `TVTheme.kt`, `TvPalette`, `TvTypography`, `TvDimens`, `TvFocus` | Look & D-pad focus |

---

## Navigation items & levels

**Items** (`NavigationItem`): `Servers`, `Settings`, `About`, `Exit`.

**Levels** (`NavigationLevel`):

| Level | Meaning |
|-------|---------|
| `Main` | Sidebar expanded |
| `Content` | Sidebar collapsed; focus in content |
| `Detail` | Server settings expanded; sidebar still visible |
| `SubScreen` | Full overlay (users / logs / certificates); sidebar hidden |

Focus helpers live on `TvNavActions` (enter/exit detail/sub-screen, pending focus flags).

---

## Content by nav item

```
TVMainScreen
├── Servers → ServersPanel
│     ├── list / select / start-stop / QR / add (TvAddServerPanel)
│     ├── detail → TvServerSettingsPanel
│     └── SubScreen
│           ├── Users → TvUsersPanel (+ TvUserEditScreen / TvUserPaneContent)
│           ├── Logs → TvLogsPanel (+ TvLogsPaneContent)
│           └── Certificates → TvCertificatesPanel
│                 └── edit → TvSslCertificatePaneContent
├── Settings → SettingsPanel / TvAppSettings
├── About → AboutPanel
└── Exit → rememberAppExit / exit flow
```

Shared chrome: `TvCards`, `TvSurfaces`, `TvBadges`, `TvOverlayShell`, `TvIcons`, `TvCollapse`.

---

## Sub-screens (`TvServerSubScreen`)

| Variant | Panel |
|---------|--------|
| `None` | — |
| `Users(serverId)` | `TvUsersPanel` |
| `Logs(serverId)` | `TvLogsPanel` |
| `Certificates(serverId)` | `TvCertificatesPanel` |

Opened from server settings actions; back uses `exitSubScreen` / `onExitToSettings`.

---

## Parity with phone

| Concern | TV note |
|---------|---------|
| Start/stop | Same fleet + permission gates; soft notification reminder |
| SAF | `SdCardRequestHandler` passed into panels |
| Free limits | Toasts via `server_limit_free` / user limit dialogs |
| Ads | Generally phone-oriented; TV free builds still use `:free` module where wired |

When changing a phone-only UX (share address, notification dialog), check whether TV needs an equivalent path.
