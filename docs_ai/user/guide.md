# User guide

How to use **WebDavFS** on phone, tablet, or Android TV.

For architecture and developer notes, see the [docs home](../README.md).

---

## 1. What is WebDavFS?

WebDavFS turns your Android device into a **file server** on your network.

- Other devices (laptop, PC, TV, another phone) can **browse, copy, and manage files** on your device.
- You can run **one server or several at once** — for example, internal storage on one port and an SD card on another.

> Shared folders on your phone that you open from a computer or other device over the network.

### Where it works by default

**On the same local network** — usually home or office **Wi‑Fi**. Both devices must be on that network. The app shows an address like `http://192.168.1.10:8080`.

### Access from the Internet (optional, advanced)

By default, people on the **Internet cannot** reach your server. If you need access from outside Wi‑Fi, you set that up yourself:

| Approach | In plain terms |
|----------|----------------|
| **Port forwarding** | Router forwards a public port to the phone’s local IP and port. Often needs a fixed public IP or **DDNS**. |
| **VPN** | Phone and remote device join the same VPN (WireGuard, Tailscale, etc.). Safer than opening the whole Internet. |
| **Tunnel** | ngrok, Cloudflare Tunnel, or a VPS reverse proxy. |

**Security:** anything on the Internet is a target. Use a **strong password** (Users), **HTTPS** when you can, avoid unnecessary write access, and prefer **VPN/tunnel** over wide-open port forwarding if unsure.

---

## 2. Several servers at once

Each server is **separate**: name, port, folder, and options. Changing one does not change the others.

**Example:** server 1 = phone storage on port `8080`, server 2 = SD card on port `8081`.

Each server needs its **own port**. If two share a port, the second will not start.

**Free edition:** up to **2** servers and **2** users. **Pro:** no fixed app-code cap on servers; Pro-only options unlocked.

**In the app:**

1. **Server list** — open the app; tap a card to open that server.  
2. **Start / Stop** — large button on the server screen; address (and QR) appear when running.  
3. **Add server** — “+” on the list; set name/port, then folder and options.  
4. **Long press** a card (or **Menu** on TV) — rename, delete; **View logs** if logging is on.  
5. **Notification** — each running server has its own; **Stop** stops only that server.

App-wide options (theme, language, permissions): **gear on the server list** → [App settings](#5-app-settings).

---

## 3. Quick start

1. Connect phone and the other device to the **same Wi‑Fi**.  
2. Open WebDavFS → tap your server.  
3. Press **Start**. Allow **storage** (and, on Android 13+, ideally **notifications**).  
4. If you share an **SD card** folder, confirm access in the system dialog.  
5. When status is **Started**, use the shown address (or **Share** / **QR**).  
6. On the other device, open that address in a browser or a WebDAV/FTP client.  
7. Press **Stop** when finished.

**Two folders at once:** add a second server with another port → set **Folder** on each → start both → connect to `:8080` and `:8081`.

---

## 4. Settings explained

Open a server on the list to change **that server’s** settings. Order matches the app: **Connection** → **Advanced** → **Autostart & logs** → actions at the bottom.

Settings in *italics* appear only under certain conditions.

### Connection

#### Port

TCP port for **this** server (e.g. `8080`). Each server needs a unique port. Change it if you see “port not available”.

#### Use password

- **Off:** anonymous access to one **Folder**; you can still set **Read only** in Advanced.  
- **On:** clients must log in; shows **Users** instead of Folder. Recommended on shared Wi‑Fi or if reachable from the Internet.

#### Folder or Users

- **Folder** (password off) — root directory this server shares.  
- **Users** (password on) — accounts, passwords, and per-user folders for **this server only**. Free: max **2** users.

#### IP

- **All (recommended):** listen on all interfaces; the link shows your current Wi‑Fi IPv4.  
- **Specific:** bind one Wi‑Fi / Ethernet / IPv6 address only.

#### SSL (https)

*WebDAV only.* Switches the address from `http://` to `https://`.

#### Certificate

*WebDAV and SSL on.* Opens the certificate library. Each server selects **one** profile; several servers can share it.

| Mode | Best for | Notes |
|------|----------|-------|
| **Auto** | Local HTTPS | Self-signed; browsers warn until you trust/export PEM. Can regenerate. |
| **Import** | Your own cert | PKCS12/JKS, or PEM + key. |
| **ACME** | Domain + Let’s Encrypt | DNS-01 TXT; renewal is **manual**; app warns before expiry. |

Restart the server if clients still see an old certificate. “Not private” with Auto is normal.

### Advanced

| Setting | Notes |
|---------|--------|
| **Server type** | **WebDAV** (default, browsers/apps) or **FTP** (`ftp://` only; hides SSL/web UI options) |
| **Buffer size** | *WebDAV.* Leave default unless you know you need it |
| **Web interface** | *WebDAV.* Browser file manager at the server URL |
| **Client UI** | *Web interface on.* Modern / Old / No JS |
| **Custom HTTP headers** | *WebDAV, Pro.* One `Name: value` per line |
| **CORS** | *WebDAV.* Cross-origin browser access; turn off if unused |
| **Read only** | *Password off.* No create/change/delete |
| **Select hidden files** | *Pro.* Paths treated as hidden |
| **Hide hidden** | *Pro.* Hide those paths from clients |
| **Show folder size** | May slow large trees |
| **Report quota in PROPFIND** | Quota properties for capable clients |

### Autostart & logs

| Setting | Notes |
|---------|--------|
| **Run with app** | Start this server when you open WebDavFS |
| **Run with device** | Start after boot (may need battery / autostart permission) |
| **Enable logging** | Default on; writes session logs under app storage |
| **Show logs in web client** | *Logging + web UI + password.* Admin can view current session in browser |
| **View logs** | Live session, past sessions, filter, export, clear/delete |

### Server actions

| Action | Effect |
|--------|--------|
| **Reset settings** | Defaults for **this** server; **name and port kept** |
| **Delete** | Stops and removes the server (and its logs) |

---

## 5. App settings

Gear icon on the **server list**.

| Setting | Notes |
|---------|--------|
| **Disable ads for session** | *Free.* Rewarded ad to hide ads until you close the app |
| **Consent to ads** | *Free.* Ad privacy / personalization form |
| **Battery optimization** | Keep the server alive with the screen off / after boot |
| **Storage permissions** | If servers cannot read files |
| **Theme** | System / Light / Dark |
| **Language** | App UI language |
| **Help** | Online help (if linked in the build) |
| **Reset all data** | Stops everything; deletes **all** servers, certificates, logs, prefs — **cannot undo** |

---

## 6. How to connect

### Browser

1. Start the server (note **http** or **https** and the **port**).  
2. Open the shown address.  
3. With **Web interface** on, you get a file manager in the browser.

### WebDAV or FTP client

Host = phone IP (or domain), **port**, protocol (**http/https/ftp**), login if **Users** is on.  
For **HTTPS + Auto**, accept the warning once or import the exported PEM.

### QR / Share

On the server screen: **QR** to scan, or tap the address to **share** / copy.

### Let's Encrypt (outline)

1. WebDAV → enable **SSL** → **Certificate** → Add → **ACME**.  
2. Domain + email → **Prepare DNS challenge** → add **TXT** at your DNS provider.  
3. **Verify and issue** → select the profile → restart if needed.  
4. Connect with `https://your-domain:port` (LAN, VPN, or after port forwarding).

---

## 7. Common problems

**Cannot connect**

- Same Wi‑Fi (unless VPN/tunnel/port forward).  
- Status **Started**; correct address, port, `http` vs `https`.  
- HTTPS: trust Auto cert or use ACME.  
- From Internet: router, firewall, phone sleep — use **battery optimization** exemption.

**Permissions**

- Allow storage in App settings.  
- For SD card, complete the system access dialog for that server’s folder.  
- On Android 13+, allow **notifications** if you want the ongoing “server running” notification (the server can still run without it).

**Port / second server won’t start**

- Unique ports (e.g. 8080 and 8081). Free: at most 2 servers.

**Wrong files**

- **Folder** is per server — open the right server on the list.

**SSL errors**

- Certificate → Status (expiry, mode). Auto: export/trust PEM. ACME: renew before expiry; DNS TXT must match.

**Stops in background**

- Battery optimization / OEM autostart (Xiaomi, Samsung, Oppo, etc.).

More technical notes: [../troubleshooting.md](../troubleshooting.md).

---

## 8. Summary

- Share folders from Android over the network — usually **home Wi‑Fi**, optionally VPN/tunnel/port forward.  
- **Server list** → each server has its own **port**, **folder**, and options.  
- **Quick start:** Wi‑Fi → Start → permissions → open the address.  
- On shared or public networks: **password**, **read-only**, and **HTTPS** when you can.

Start with one server and defaults on home Wi‑Fi. Add another server with a new port when you need a second shared folder.
