# TV and ads (notes)

End-user TV map: [tv-ui-map.md](tv-ui-map.md). Free ads UX: [free-vs-pro.md](free-vs-pro.md).

## Android TV

Same multi-server / SSL / users / logs ideas as phone, but navigation is leanback (Servers / Settings / About / Exit). There is **no** phone-style Status metrics tab or bell inbox — announcements may appear as blocking dialogs.

Permissions (storage, notifications, battery) still matter so the server can start and stay alive.

## Ads (Free flavor)

| Piece | What users see |
|-------|----------------|
| Banners | On phone Free UI while ads are enabled |
| Interstitials | Occasional fullscreen ads after several server starts (with cooldowns) |
| Consent | App settings → Application → consent form when offered |
| Disable for session | App settings → watch rewarded video → ads off until the app is closed |

Pro builds have no ads. TV Free builds may include the same flavor code; ad surfaces are mostly phone-oriented.
