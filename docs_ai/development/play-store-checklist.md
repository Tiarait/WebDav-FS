# Play / store release checklist

Companion to [testing-release.md](testing-release.md). For FGS subtype wording, use the checklist below; a longer Play Console draft may exist as `docs/PLAY_FGS_SPECIAL_USE.md` in the full source repository.

Use when preparing a Play Console upload (Free and/or Pro).

---

## App identity

- [ ] Correct package: Free `ua.tiar.webdavfs` / Pro `ua.tiar.webdavfspro`
- [ ] `versionCode` / `versionName` bumped in `app/build.gradle.kts`
- [ ] Signed release build; ProGuard mapping kept if needed for crash deobfuscation
- [ ] Both flavors tested if both are published

---

## Foreground service

- [ ] Manifest: `foregroundServiceType="specialUse"` + `FOREGROUND_SERVICE_SPECIAL_USE`
- [ ] `PROPERTY_SPECIAL_USE_FGS_SUBTYPE` matches Play declaration, e.g.  
  *User-started local WebDAV/FTP file server that must remain reachable on the LAN while the screen is off until the user stops it. Not covered by dataSync (time-limited) or connectedDevice.*
- [ ] Play Console form filled (suggested paste below)
- [ ] Do **not** declare as `dataSync` for this product

### Suggested Play Console paste — what the FGS does

```
WebDavFS turns the Android device into a local WebDAV and/or FTP file server on the Wi‑Fi / LAN. When the user starts a server, NotificationService runs as a foreground service so the HTTP/HTTPS/FTP listener stays available while the screen is off or the app is in the background. The service shows an ongoing notification with the server address and a Stop action. The user can also enable optional autostart with the app or after device boot. The service stops when the user stops all servers (or via the notification Stop action).
```

### Why not `dataSync` / `connectedDevice`

- `dataSync` is time-limited on recent Android versions — unsuitable for a user-started LAN server that may run for days.
- `connectedDevice` is for BT/USB/NFC-style peripherals — not this use case.

A longer draft may exist as `docs/PLAY_FGS_SPECIAL_USE.md` in the full source repository.

---

## Permissions (Data safety / declarations)

Align Console answers with what the app actually does:

| Area | App behavior (summary) |
|------|-------------------------|
| Files / photos | Local file server; all-files / SAF access to serve user-chosen storage |
| Notifications | FGS ongoing notification; runtime `POST_NOTIFICATIONS` on API 33+ |
| Network | LAN (and optional user-exposed) server sockets; ACME/DNS if used |
| Ads (Free) | AdMob + UMP consent |
| Approximate location | Typically **not** used for tracking — verify before claiming |
| Account / personal info | Optional WebDAV users stored **on device**; ACME email if user enters it |

- [ ] Data safety form updated after permission / ads / ACME changes
- [ ] Privacy policy URL current (if required by ads / data collection)

---

## Ads (Free only)

- [ ] AdMob app id in free manifest / gradle matches Console app
- [ ] UMP / consent flow tested in EEA-like conditions
- [ ] Interstitial rules still acceptable (frequency; not on every tap)
- [ ] Pro APK has **no** AdMob dependency

---

## Content / policy

- [ ] Store listing does not promise cloud sync or remote admin beyond LAN/server reality
- [ ] Screenshots include phone and TV if Leanback is marketed
- [ ] “Sensitive permissions” justifications ready (all-files access, FGS)

---

## Pre-upload smoke

Re-run matrix in [testing-release.md](testing-release.md), plus:

- [ ] Fresh install → deny notifications → server still starts + soft reminder
- [ ] Boot autostart on at least one OEM device
- [ ] HTTPS Auto cert + one Import or ACME path if advertised
