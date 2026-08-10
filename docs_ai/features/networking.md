# Networking (users)

How addresses and bind options work. Setup walkthrough: [../user/guide.md](../user/guide.md).

## Address on Home

When the server is **Started**, Home shows something like `http://192.168.x.x:8080` (or `https://…`). Use **Connect** for QR, copy, and share.

- **IPv6** addresses appear in brackets in the URL when used.  
- If you only see `127.0.0.1`, the device has no usable LAN IPv4 (Wi‑Fi off, odd AP mode, etc.).

## IP setting (Connection)

| Choice | Meaning |
|--------|---------|
| **All** (recommended) | Listen on all interfaces; the link shows your current Wi‑Fi IPv4 |
| A listed address | Bind **only** that interface |
| `127.0.0.1` | Only apps on **this device** can connect |

Wrong bind = PC cannot connect even though the server is Started. Prefer **All** on home Wi‑Fi.

Changing IP / port / SSL while running may show a **Restart** banner — restart so the listener matches the new settings.

## Buffer size (WebDAV Advanced)

Transfer buffer for WebDAV. Leave **Recommended** unless large transfers on a fast LAN feel slow. Very high values can use more memory and may be less stable on low-RAM phones (the app warns).

## Wi‑Fi change

If Wi‑Fi or IP changes while servers are running:

1. The shown address updates.  
2. Running servers may **auto-restart** briefly so they bind the new network — in-flight transfers can fail; reconnect clients.  
3. If the address still looks wrong, stop/start once from Home.

## Same LAN checklist

- Phone and PC on the same Wi‑Fi (or VPN).  
- Correct **port** and `http` vs `https`.  
- OEM “firewall” / data saver not blocking the app.  
- Battery optimization exempt if the server dies when the screen is off — App settings → Battery.

Related: [../troubleshooting.md](../troubleshooting.md), [../development/oem-battery.md](../development/oem-battery.md) (OEM battery screens).
