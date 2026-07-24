# Manual test & release checklist

> Pass G starter. Expand into automated tests when the project adds them.

## Before a release build

- [ ] Version bump in `app/build.gradle.kts` (`versionMajor/Minor/Patch`)
- [ ] `assembleFreeRelease` and/or `assembleProRelease` with local signing props
- [ ] Web client deployed if UI changed: `cd client && npm run deploy`
- [ ] New strings merged for all locales (`scripts/merge_locale_strings.py`)
- [ ] FGS still `specialUse` + Play form text matches your Play Console declaration (see [play-store-checklist.md](play-store-checklist.md))
- [ ] No secrets in git (`gradle.properties.local`, keystores)

## Smoke matrix (manual)

| Case | Free | Pro | Phone | TV |
|------|------|-----|-------|-----|
| Start / stop WebDAV HTTP | ☐ | ☐ | ☐ | ☐ |
| Start HTTPS (Auto cert) | ☐ | ☐ | ☐ | ☐ |
| FTP start / stop | ☐ | ☐ | ☐ | ☐ |
| Second server (within free cap / pro) | ☐ | ☐ | ☐ | ☐ |
| Use password + users CRUD | ☐ | ☐ | ☐ | ☐ |
| Share address / QR | ☐ | ☐ | ☐ | ☐ |
| Deny notifications → soft reminder, server still runs | ☐ | ☐ | ☐ | — |
| Wi‑Fi toggle while running | ☐ | ☐ | ☐ | ☐ |
| Boot autostart (`startWithDevice`) | ☐ | ☐ | ☐ | ☐ |
| SAF / SD write grant | ☐ | ☐ | ☐ | ☐ |
| Ads path (free only) | ☐ | — | ☐ | — |
| Web UI modern client browse/upload | ☐ | ☐ | ☐ | — |

## Regression hotspots

- WakeLock released while Starting/Stopping
- Ads observing only default server
- Locale apostrophes (French) breaking resource merge
- Free+Pro both installed → separate notifications/ports
