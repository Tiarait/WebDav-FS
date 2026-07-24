# Modules

Gradle includes (`settings.gradle.kts`): `:app`, `:free`, `:pro`, `:acme`.  
`client/` is an **npm** project, not a Gradle module.

## `:app`

Path: `app/`

Main Android application:

- Compose phone UI + TV UI
- `ServerFleet`, Ktor WebDAV, FTP
- `NotificationService` (FGS)
- Koin DI (`di/AppModule.kt`, `di/ServerModule.kt`)
- Flavor-specific manifests under `app/src/free/` and `app/src/pro/`

Dependencies:

- Always: `implementation(project(":acme"))`
- Flavor: `freeImplementation(project(":free"))` / `proImplementation(project(":pro"))`

## `:free`

Path: `free/`

Free-tier library:

- AdMob / UMP wiring
- `ExtendUtils` with `isPro() == false`
- Persistence helpers used by free builds (users, etc.)

## `:pro`

Path: `pro/`

Pro-tier library:

- `ExtendUtils.isPro() == true`
- Real implementations for Pro-only persistence (e.g. hidden file lists)
- No ad dependencies

## `:acme`

Path: `acme/`

Let's Encrypt integration (acme4j): certificate issuance helpers used when SSL mode is **ACME**. Product path emphasizes DNS-01; HTTP-01 helpers also exist in code for challenges.

## `client/` (web UI)

Path: `client/`

Sources for the in-server browser UI. Production deploy:

```bash
cd client && npm run deploy
```

Copies bundled assets into `app/src/main/assets/client/`. Served by Ktor under `/~assets~/` (not a WebDAV path).

See `client/README.md` and [../features/web-client.md](../features/web-client.md).

## `scripts/`

Path: `scripts/`

- `extra_translations.py` — batch string translations
- `merge_locale_strings.py` — merge into `app/src/main/res/values-*/strings.xml`

See [../development/localization.md](../development/localization.md).
