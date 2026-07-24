# Troubleshooting

Quick symptoms → likely causes. End users can start with [user/guide.md § Common problems](user/guide.md#7-common-problems).  
Prefer in-app **Logs** (when logging is enabled) when debugging further.

---

## Notifications

| Symptom | Likely cause | What to check |
|---------|--------------|---------------|
| No persistent notification while server runs | `POST_NOTIFICATIONS` denied (API 33+) | Soft reminder / system app notification settings |
| FGS crashes / Play rejection | Wrong FGS type | Must be `specialUse` + subtype property — see Play checklist |
| Notification actions do nothing | PendingIntent / package mismatch | Free vs Pro are different `applicationId`s |

---

## Server will not start

| Message / symptom | Cause |
|-------------------|--------|
| `Port {n} is already in use` | Another process (or another profile) bound the port |
| `Port {n} is not available` | Probe failed after retries |
| `Access denied for port {n}` | `AccessControlException` / restricted bind |
| `Root folder … is not exist.` | Missing root on internal storage (SD missing may still attempt start) |
| `Cant generate ip address` | Empty host after retries |
| `FTP Server Configuration Error: …` | Apache FtpServer config failure |
| Toast: already running on port | Same app already serving; treated as Started |
| Start opens permission UI | `UtilsPermission.hasPermissions` false |

Also: free tier cannot add endless servers (`FREE_MAX_SERVERS = 2`).

---

## Wrong / unreachable address

| Symptom | Check |
|---------|--------|
| Shows `127.0.0.1` | No usable LAN IPv4; Wi‑Fi off / AP-only quirks |
| Client on PC cannot connect | Same LAN? Client VPN? `ipType` bound to wrong interface? Firewall on phone OEM? |
| Address stale after Wi‑Fi change | `WifiReceiver` should refresh + restart; verify receiver registered and logs |
| HTTPS trust errors on PC | Auto/self-signed cert — expect browser warning; import/ACME for real CA |

---

## Links / share

| Symptom | Cause |
|---------|--------|
| “No application found” opening http(s) | Android 11+ package visibility — app uses `<queries>` + share sheet; DialogLink should `startActivity` without relying on `resolveActivity` alone |
| Share sheet missing targets | Rare OEM; copy from share targets or clipboard |

---

## Users / auth

| Symptom | Check |
|---------|--------|
| Cannot add user | Free cap `FREE_MAX_USERS = 2` |
| 401 from WebDAV | `useUsers` on; credentials vs `servers_data/{id}/users` |
| Anonymous unexpectedly | `useUsers == false` |

---

## SD card / write

| Symptom | Check |
|---------|--------|
| Cannot write external SD | SAF tree grant missing — re-pick folder via `SdCardRequestHandler` |
| “No app can do this” | No `OPEN_DOCUMENT_TREE` handler / all-files fallback |
| Folder reset dialog | Root path invalid — `root_folder_not_exist_reset_dialog` flow |

---

## Ads (free)

| Symptom | Check |
|---------|--------|
| Interstitial never shows | `countServerStart` threshold, cooldown, `areAdsEnabled`, UI visible; status observer must watch **all** fleet servers |
| Ads on Pro | Should not — wrong flavor / dependency |

---

## Process dies in background

| Check |
|-------|
| Battery optimization / OEM autostart |
| WakeLock released while servers still active (bug — `anyActive()`) |
| User swiped away with stop-on-remove OEM behavior |
| `dataSync` FGS timeout — must not use `dataSync` for this product |

---

## Related docs

- [architecture/runtime-lifecycle.md](architecture/runtime-lifecycle.md)
- [features/networking.md](features/networking.md)
- [development/permissions-saf.md](development/permissions-saf.md)
- [user/guide.md](user/guide.md) (end-user LAN vs internet)
