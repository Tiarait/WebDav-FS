# Battery optimization & OEM autostart

Long-running LAN servers get killed by aggressive battery savers. Grant exemptions from **App settings → Permissions → Battery optimization**, and (on many phones) from the manufacturer’s own “autostart” / “background” screens.

---

## What users should do

1. In WebDavFS: **App settings** (gear) → **Permissions** → **Battery optimization** — allow unrestricted / ignore optimizations when asked.  
2. If **Run with device** (boot) still fails, open the phone’s system settings for this app and enable **Autostart**, **Allow background activity**, or similar.  
3. Brands that often need an extra step: **Xiaomi / Redmi / Poco**, **Samsung**, **Oppo / ColorOS**, **Huawei**, **Meizu**, **HTC**, **ZTE**, **Letv**. Menus change by OS version — search Settings for the brand’s “autostart” or “battery” page if the in-app deep link does nothing.  
4. Android TV: same idea; some TVs have a separate “apps that can run in background” list.

Foreground service + wake lock help, but they **do not replace** OEM allowlisting on aggressive skins.

Related: [../troubleshooting.md](../troubleshooting.md), [../user/guide.md](../user/guide.md#9-app-settings-gear-on-the-list).

---

## Maintainer notes

In-app entry points call battery exemption intents, then brand-specific screens when available. Probe whether a brand path exists before offering a deep link; always keep a fallback to system battery / app details. OEM UIs change often — update this page when adding brands.
