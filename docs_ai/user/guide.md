# User guide

How to use **WebDavFS** on phone, tablet, or Android TV.

DeepWiki / docs home: [../README.md](../README.md).

---

## 1. What is WebDavFS?

WebDavFS turns your Android device into a **file server** on your network.

- Other devices can **browse, copy, and manage files** on your phone or TV.
- You can run **one server or several** — for example internal storage on one port and an SD card on another.
- WebDAV clients get extras like **thumbnails**, **locks**, **sync**, and **quota** — see [../features/webdav-capabilities.md](../features/webdav-capabilities.md).

### Where it works by default

**Same local network** (home/office Wi‑Fi). The app shows an address like `http://192.168.1.10:8080`.

### Access from the Internet (optional)

By default the Internet cannot reach your server. If you need remote access, you set it up yourself (port forward, VPN, or tunnel). Prefer **password**, **HTTPS**, and **VPN/tunnel** over open port forwarding.

---

## 2. App layout (phone)

| Place | What you do there |
|-------|-------------------|
| **Server list** | All servers; play/stop on the card; **+** to add; **gear** for app settings; **bell** for in-app notices |
| **Home** tab | Large start/stop, address, connect / QR / share |
| **Settings** tab | Options for **this** server only |
| **Status** tab | Live metrics: session, mode flags, device, transfers |
| **Logs** tab | Live and saved session logs (if logging is on) |

Android TV uses a D-pad shell with the same ideas — [../features/tv-ui-map.md](../features/tv-ui-map.md).

### Several servers

Each server has its own **port**, **folder** (or users), and options.

1. Tap a card to open it.  
2. Start/stop on the card or on **Home**.  
3. **+** to add (Free: max **2** servers).  
4. Overflow / long-press (Menu on TV): rename, delete, open logs.  
5. System **notification** while running: address + stop one or all.

---

## 3. Quick start

1. Same Wi‑Fi on phone and PC.  
2. Open WebDavFS → your server.  
3. **Start** — allow **storage**; on Android 13+ allow **notifications** if you want the ongoing notification.  
4. SD card folder → confirm the system access dialog when asked (prefer selecting the **SD root**, then Select).  
5. When **Started**, open **Connect** for QR / copy / share, or type the address on the other device.  
6. **Stop** when finished.

**First launch (onboarding):** welcome → permissions one-by-one (storage → notifications → battery; you can Skip and grant later) → name/port → folder → Ready → open the server → Start.

---

## 4. Choosing a folder (quick paths)

When you pick a share folder (settings, add server, users, onboarding), the picker has two useful parts:

### Quick Access

Shortcuts (when storage permission allows), for example:

- Internal storage  
- SD card (if present)  
- Downloads, Gallery / Pictures, Documents, Movies, Music  

Tap a shortcut to jump there instead of digging from the device root.

### Browse

Navigate folders normally. You **select folders**, not individual files (files may be shown only as orientation).

After you pick an SD or “Documents” style location, Android may ask for **folder access** (SAF) — accept it so the server can read/write that tree.

**SD write tip:** if uploads to the card fail or look read-only, open the system picker again, go to the **root of the SD card**, then tap **Select** / **Use this folder** (not a deep subfolder first). Re-start the server afterward if it was already running.

---

## 5. Server settings

Open a server → **Settings**. Groups: **Connection** → **Advanced** → **Autostart & logs** → actions.

### Connection

| Setting | Notes |
|---------|--------|
| **Port** | Unique per server (e.g. `8080`) |
| **Use password** | Off = one Folder (+ optional Read only). On = **Users** logins |
| **Folder / Users** | What clients see; Free max **2** users |
| **IP** | **All** (recommended) or bind one interface |
| **SSL (https)** | *WebDAV* — then pick a **Certificate** (Auto / Import / ACME) |

### Advanced

| Setting | Notes |
|---------|--------|
| **Web interface** | Browser file manager at the server URL |
| **Client UI** | *Web interface on.* **WebDavFS client** (default), Old Modern UI, Old UI, or No JS UI |
| **Custom HTTP headers** | *WebDAV, Pro* |
| **CORS** | Cross-origin browser access |
| **File metadata cache** | On by default; speeds listings |
| **Read only** | *Password off* |
| **Select hidden files** | *Pro.* Choose paths treated as hidden from clients |
| **Hide hidden** | *Pro.* Also hide names starting with a dot |
| **Show folder size** | May slow large trees |
| **Report quota in PROPFIND** | Quota for capable clients (can slow deep listings) |
| **Buffer size** | *WebDAV.* Leave **Recommended** unless you know you need more |
| **Server type** | **WebDAV** or **FTP** — see [../features/ftp.md](../features/ftp.md) |

WebDAV protocol extras: [../features/webdav-capabilities.md](../features/webdav-capabilities.md).

### Autostart & logs

| Setting | Notes |
|---------|--------|
| **Run with app** | Start this server when you open WebDavFS |
| **Run with device** | Start after boot (often needs battery / OEM autostart permission) |
| **Enable logging** | Default on — writes session files |
| **Show logs in web client** | *Logging + web UI + password* — admin can view the current session in the browser |

### Server actions

| Action | Effect |
|--------|--------|
| **Reset settings** | Defaults for **this** server; **name and port kept** |
| **Delete** | Stops and removes the server (and its logs) |

---

## 6. Status tab (metrics)

Open the server → **Status**. Cards update while the server runs.

### Session

- Running / stopped / starting / error  
- Uptime while running  
- **Listen** — protocol and port (e.g. HTTP · 8080)

### Mode (chips)

Lit when enabled:

| Chip | Meaning |
|------|---------|
| **Auth** | Password / users on |
| **RO** | Read only |
| **Web** | Browser web interface on |
| **CORS** | CORS headers on |
| **Cache** | File metadata cache on |

When Cache is on and the server is running, you may also see **Meta** / **List** cache occupancy.

### Activity

While running: approximate **clients**, **requests/min**, **errors/min** (from recent traffic).

### Transfers

Active uploads/downloads with progress when transfers are in flight.

### Device

Phone health-style metrics (availability depends on the device):

- Battery %  
- Temperature  
- Free RAM  
- CPU (when available)  
- Thermal state  
- Free storage  

### Restart banner

If you change options that need a new listener (port, SSL, web UI, CORS, custom headers, …) while the server is **running**, Home/Status may ask you to **Restart** so clients see the new setup. Cache on/off does **not** require restart.

---

## 7. Logs

Open the server → **Logs** (or **View logs** from the list menu when logging is on).

| Feature | What it does |
|---------|----------------|
| **Current session** | Live lines while the server runs |
| **Pause / resume live** | Freeze the stream to read without jumping |
| **Saved sessions** | Older log files from previous runs |
| **Search** | Find text in the session |
| **Filter** | By level, HTTP method, status class (2xx / 4xx / …) |
| **Copy** | Clipboard |
| **Export / share** | File you can send for support |
| **Clear current** | Wipe the live session buffer |
| **Delete session / delete all** | Remove saved files |

Turn logging off in Settings if you do not want files written. For browser admins, enable **Show logs in web client**.

---

## 8. Notifications (two kinds)

### System notification (server running)

While any server is started, Android shows an ongoing notification (address, stop).  
Allow **Notification permissions** in App settings on Android 13+. Denying it does **not** stop the server; you just may not see the ongoing entry. The app may remind you once.

### In-app notifications (bell)

On the **server list**, the **bell** opens an inbox of product notices (tips, links, rate prompts, important messages). You can refresh, open links, and dismiss. Unread count appears on the icon. This is separate from the system “server running” notification.

---

## 9. App settings (gear on the list)

### Application

| Setting | Notes |
|---------|--------|
| **Theme** | System / Light / Dark |
| **Language** | App UI language |
| **Disable ads for this session** | *Free.* Confirm → watch a rewarded video → banners and fullscreen ads stay off **until you close the app** (not saved) |
| **Consent to ads** | *Free.* Privacy / personalization form when offered |

Details: [../features/free-vs-pro.md](../features/free-vs-pro.md).

### Permissions

| Setting | Notes |
|---------|--------|
| **Battery optimization** | Keep the server alive with the screen off / after boot. On Xiaomi, Samsung, Oppo, Huawei, etc. also allow **autostart** / “no battery restriction” in the phone’s own settings — [../development/oem-battery.md](../development/oem-battery.md) |
| **Storage / folder permissions** | Needed to share files |
| **Notification permissions** | Ongoing “server running” notification (Android 13+) |

### System

| Item | Notes |
|------|--------|
| **About** | Version, blurb; website, email, Play Store, GitHub |
| **Feedback** | Email the developer |
| **Help** | Docs / help link |
| **Ask AI** | DeepWiki Q&A |
| **Reset all data** | Deletes **all** servers, certs, logs, prefs — cannot undo |

---

## 10. How to connect

### Browser

Start the server → open the address. With **Web interface** on you get the file manager (thumbs, menus, optional ZIP — [../features/web-client.md](../features/web-client.md)).

### WebDAV or FTP client

Host = phone IP (or domain), **port**, `http`/`https`/`ftp`, login if Users is on.  
FTP mode: [../features/ftp.md](../features/ftp.md).

### Connect sheet

**Home** → Connect / QR: QR code, **Copy link**, **Share**, open in browser.

### Certificates (HTTPS)

Auto: regenerate / **export PEM** to trust on a PC. Import: keystore or PEM+key. ACME: DNS TXT → issue. Full tap path: [../features/ssl-certificates-flow.md](../features/ssl-certificates-flow.md).

### Let's Encrypt (outline)

1. WebDAV → SSL → Certificate → **ACME**.  
2. Prepare DNS → add **TXT** → verify / issue.  
3. Use `https://your-domain:port`.

---

## 11. Other useful bits

| Feature | Tip |
|---------|-----|
| **Wi‑Fi change** | Address updates; running servers may briefly auto-restart — reconnect clients ([networking.md](../features/networking.md)) |
| **Onboarding** | Permissions one-by-one, then name/port/folder |
| **Add server (app)** | Name, port, optional password, folder |
| **Add server (browser)** | Sidebar can open *other* WebDAV URLs via this phone — [web-client.md](../features/web-client.md) |
| **Users roles** | Admin needed for web logs — [auth-users.md](../features/auth-users.md) |
| **Certificate library** | Shared; export/regen/ACME — [ssl-certificates-flow.md](../features/ssl-certificates-flow.md) |
| **FTP** | Switch server type — [ftp.md](../features/ftp.md) |
| **Android TV** | [tv-ui-map.md](../features/tv-ui-map.md) |
| **Free ads** | Rewarded disable for session — [free-vs-pro.md](../features/free-vs-pro.md) |
| **Ask AI / Help** | App settings → System |

---

## 12. Common problems

**Cannot connect** — same Wi‑Fi; status Started; correct port and http/https; battery optimization if it dies in the background.

**Permissions** — storage in App settings; SAF for SD; notifications for the ongoing entry (optional).

**Port / second server** — unique ports; Free max 2 servers.

**Wrong files** — Folder is per server.

**SSL** — Auto warns in browsers until trusted; ACME needs correct DNS TXT.

**Logs empty** — logging enabled? correct server? try Current session vs a saved session.

More: [../troubleshooting.md](../troubleshooting.md).

---

## 13. Summary

- List → Home / Settings / **Status** / **Logs**.  
- Quick folder shortcuts; for SD writes pick the **card root** then Select.  
- Metrics on Status; rich Logs tools; bell for in-app notices; system notification while running.  
- HTTPS: Auto / Import / ACME — [../features/ssl-certificates-flow.md](../features/ssl-certificates-flow.md).  
- Free: ads can be disabled for the session via rewarded video; max 2 servers / 2 users.  
- Prefer password + HTTPS on shared or public networks.
