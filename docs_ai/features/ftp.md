# FTP mode (users)

WebDAV is the default. You can switch a server to **FTP** for classic FTP clients.

## Where to tap

Server → **Settings** → **Advanced** → **Server type** → **FTP**.

Then start the server and connect with an FTP app to:

`ftp://PHONE_IP:PORT`

## What changes vs WebDAV

| Feature | On FTP |
|---------|--------|
| HTTPS / certificates | Hidden — FTP here is plain `ftp://` |
| Web interface / Client UI | Hidden |
| Buffer size / CORS / WebDAV cache | WebDAV-oriented options go away |
| Users / password | Still available — **strongly recommended** |

Prefer **Use password** + Users. Anonymous FTP on Wi‑Fi is unsafe.

## Limits to expect

- Best on **LAN**. Passive FTP (PASV) through home NAT/port-forward is often painful; the app does not expose a custom PASV port range.
- Free/Pro server and user caps still apply.
- Thumbnails, sync-collection, ZIP-on-folder, and other WebDAV extras are **WebDAV-only**.

## Tip

Need browser UI + HTTPS + modern clients → stay on **WebDAV**. Use FTP only when a device/app speaks FTP and nothing else.
