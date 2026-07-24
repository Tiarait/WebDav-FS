# Development

## Index

| Doc | Topic |
|-----|--------|
| [entry-points.md](entry-points.md) | App, Activity, Boot, Service |
| [permissions-saf.md](permissions-saf.md) | Storage, notifications, SAF, battery |
| [oem-battery.md](oem-battery.md) | OEM autostart / battery helper brands |
| [di.md](di.md) | Koin AppModule / ServerModule |
| [notifications-intents.md](notifications-intents.md) | FGS commands, extras, PendingIntents |
| [localization.md](localization.md) | `scripts/` string merge |
| [testing-release.md](testing-release.md) | Manual smoke / release checklist |
| [play-store-checklist.md](play-store-checklist.md) | Play Console / Data safety reminders |
| [conventions.md](conventions.md) | Working agreements for contributors |

## Quick commands

```bash
# App
./gradlew :app:assembleFreeDebug

# Locales (from repo root)
cd scripts && python3 merge_locale_strings.py

# Web client
cd client && npm run deploy
```

Also see [../troubleshooting.md](../troubleshooting.md).