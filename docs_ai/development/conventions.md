# Conventions

## Documentation

- **`docs_ai/`** — publishable documentation (user guide + technical). English.  
- **`docs/`** — additional long-form material in the same repo (SSL design, Play FGS copy, JS APIs). Prefer linking rather than duplicating huge essays.  
- When code and docs disagree, **fix code**, then update docs.

## Code changes

- Match existing style; minimal diffs; no drive-by refactors.  
- Free vs Pro: keep flavor boundaries (`:free` / `:pro`, `UtilsPro`, `BuildConfig.IS_PRO`).  
- Do not commit secrets (`gradle.properties.local`, keystores, API keys).

## Server / FGS safety

- Do not switch FGS back to `dataSync` without a new platform strategy.  
- Do not release WakeLock while `anyActive()` is true.  
- Do not call `stopAll()` from `NotificationService.onDestroy` unless deliberately redesigned.

## UI

- Phone Compose and TV are separate; fix both when changing start/share/permission UX.  
- Notification permission: soft reminder OK; do not spam the system dialog every start.
