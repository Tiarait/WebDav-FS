# TV UI and ads

## Android TV / Leanback

- Manifest declares leanback launcher / touchscreen optional.
- `MainActivity` branches on `UiUtils.isRunningOnTv` → `TVMainScreen` instead of phone Compose home.
- TV packages under `ui/tv/`: navigation panels, server list, settings, users, logs, certificates, QR.

Permissions and start/stop still go through `UtilsPermission` and `ServerListViewModel` / detail VMs.

Soft notification reminders also apply on TV when posting notifications is denied (Android 13+).

## Ads (free flavor)

| Piece | Notes |
|-------|--------|
| Module | `:free` |
| Init | Free `MainActivity` path / AdMob initialize helpers |
| Surfaces | Banners in Compose; interstitial rules tied to server start/stop counts and cooldowns |
| Consent | UMP / consent flows where wired in free module |
| Session | User may disable ads for the current session (settings) |

Pro builds depend on `:pro` and must not pull AdMob.

When changing ad triggers, observe **all** fleet servers’ status flows (not only the default server) so multi-server start/stop is counted correctly.
