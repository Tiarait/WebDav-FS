# Battery optimization & OEM autostart

Long-running LAN servers are sensitive to OEM battery savers. The app asks for exemptions from **app settings** (and related TV paths).

---

## Standard Android path

`PermissionUtils.requestBatteryOptimizationExemption`:

1. Prefer `ACTION_REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` for this package.
2. Fallbacks: battery optimization list / app details.
3. Then OEM-specific screens via **`BatteryPermissionHelper`**.

`PermissionUtils.openBatterySettings` opens manufacturer or system battery UIs (also used from server extras when enabling autostart).

---

## Brands handled in `BatteryPermissionHelper`

Based on `Build.BRAND` (lowercase):

| Brand key(s) | Typical target |
|--------------|----------------|
| `htc` | `com.htc.pitroad` landing |
| `huawei` | `huawei.intent.action.HSM_PROTECTED_APPS` |
| `letv` | Letv safe background manager |
| `meizu` | Meizu safe power UI |
| `oppo` | ColorOS power consumption activities |
| `samsung` | Device care / battery activities (`com.samsung.android.lool` / CN fallback) |
| `xiaomi`, `poco`, `redmi` | MIUI powerkeeper / autostart (`HiddenAppsConfigActivity`, `AutoStartManagementActivity`, …) |
| `zte` | HeartyService clear-app settings |
| *else* | Generic default intents |

`isBatterySaverPermissionAvailable` probes whether a brand-specific path exists before offering deep links.

**Caveat:** OEM UIs change often; intents may no-op on new OS versions. Always keep a fallback to system battery settings. Document new brands in code + this page when adding them.

---

## Product guidance

- Enabling **start with device** / **start with app** should nudge users toward unrestricted battery where possible.
- TV: separate logging tag (`TvBattery`) / TV-oriented request path in `PermissionUtils`.
- FGS + WakeLock help, but **do not replace** OEM autostart allowlisting on aggressive skins (MIUI, ColorOS, …).

Related: [permissions-saf.md](permissions-saf.md), [../troubleshooting.md](../troubleshooting.md) (“Process dies in background”).
