# Free vs Pro

## Packages

| | Free | Pro |
|---|------|-----|
| Application id | `ua.tiar.webdavfs` | `ua.tiar.webdavfspro` |
| Flavor module | `:free` | `:pro` |
| Ads | Yes (AdMob + UMP) | No |
| `IS_PRO` / `ExtendUtils.isPro()` | false | true |

Both can coexist on one device.

## Caps (code)

| Limit | Free | Pro |
|-------|------|-----|
| Server profiles | `ServerFleet.FREE_MAX_SERVERS` (**2**) | No fixed app-code cap |
| Users | `UtilsPro.FREE_MAX_USERS` (**2**) | No fixed free-style cap in Pro helpers |

Always re-read constants before documenting numbers in user-facing copy.

## Gated features (examples)

Pro-oriented capabilities (gated via `UtilsPro` / profile apply):

- Hidden-files configuration persistence
- Custom HTTP headers (global/custom header lists)

Exact gates live in settings lists and `Server.applyProfile` — search for `UtilsPro.isPro` / `ExtendUtils`.

## Ads (free only)

- Init from `MainActivity` / AdMob helpers in `:free`
- Banners and interstitial/rewarded flows with session rules (e.g. after several server starts, cooldowns)
- User can disable ads for the session / manage consent where implemented

See [tv-and-ads.md](tv-and-ads.md).
