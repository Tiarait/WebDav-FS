# Troubleshooting

Symptoms → what to try. Start with [user/guide.md](user/guide.md). Use in-app **Logs** when logging is enabled.

---

## Notifications

| Symptom | Try |
|---------|-----|
| No ongoing “server running” entry | Allow notification permission (Android 13+); App settings → Permissions |
| Bell inbox empty | Pull refresh in the inbox; not the same as the system notification |
| Stop in notification does nothing | Confirm Free vs Pro package; reopen the app and stop from Home |

---

## Server will not start

| Symptom | Try |
|---------|-----|
| Port already in use | Pick another port; stop the other server/app using it |
| Root folder missing | Re-pick Folder (Quick Access / Browse); SD may need SAF again |
| Permission UI on start | Grant storage; for SD complete the system folder access dialog |
| Cannot add another server | Free max **2** servers |

---

## Wrong / unreachable address

| Symptom | Try |
|---------|-----|
| Shows localhost only | Turn on Wi‑Fi / Ethernet; avoid bad VPN |
| PC cannot connect | Same LAN? Correct port and http/https? OEM firewall? |
| Address wrong after Wi‑Fi change | Wait for refresh or restart the server; check Home address |
| HTTPS warnings | Normal for Auto/self-signed; use ACME or trust/export the cert |

---

## Folder picker / SD

| Symptom | Try |
|---------|-----|
| Shortcuts missing | Grant storage first |
| Cannot write on SD | Re-open the system picker, go to the **SD card root**, tap **Select** / Use this folder, accept access, then restart the server |
| “No app can do this” | Use all-files access or another picker path from Quick Access |

---

## Logs

| Symptom | Try |
|---------|-----|
| Empty Logs tab | Enable logging in Settings; open **Current session** |
| Too much noise | Filter by status/method; search; pause live |
| Need to send for help | Export / share from the Logs menu |

---

## Users / auth

| Symptom | Try |
|---------|-----|
| Cannot add user | Free max **2** users |
| 401 from clients | Password mode on; check login/password |
| Unexpected anonymous access | Turn **Use password** on |

---

## Ads (Free)

| Symptom | Try |
|---------|-----|
| Ads still show after reward | Session-only disable - restarting the app brings ads back |
| Consent / personalized ads | App settings → Application → consent |

---

## SSL

| Symptom | Try |
|---------|-----|
| Clients reject cert | Auto is self-signed; import PEM or use ACME |
| ACME fails | DNS TXT must match; wait for propagation; retry verify |

## Dies when screen is off / after reboot

| Symptom | Try |
|---------|-----|
| Server stops in background | App settings → Battery optimization; allow unrestricted |
| Boot start does nothing | Enable **Run with device**; also OEM autostart (Xiaomi, Samsung, Oppo, …) — [development/oem-battery.md](development/oem-battery.md) |

## FTP

| Symptom | Try |
|---------|-----|
| No web UI / HTTPS options | Expected in FTP mode — switch **Server type** back to WebDAV if you need them |
| Client cannot connect | Same LAN; `ftp://IP:port`; prefer password mode — [features/ftp.md](features/ftp.md) |

Related: [features/ssl.md](features/ssl.md), [features/app-ux.md](features/app-ux.md), [features/networking.md](features/networking.md).
