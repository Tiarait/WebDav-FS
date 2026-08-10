# Browser file manager (web UI)

When **Web interface** is on (server Settings → Advanced), open the server URL in a browser for a file manager.

## Basics

- Browse, upload, download, create folders, rename/move (subject to permissions).
- **Thumbnails** for many images/videos.
- Optional **logs** for admins when “Show logs in web client” is on (password mode + **Admin** user).
- **Client UI** setting: **WebDavFS client** (default), **Old Modern UI**, **Old UI**, or **No JS UI** (simple listing for very old browsers).

## Download a folder as ZIP

The built-in UI may not always show a big “ZIP this folder” button. Compatible ways:

| Method | How |
|--------|-----|
| Query on the folder URL | Open/add `?zip=1` on a **folder** URL (or `accept=zip`) → download `FolderName.zip` |
| HTTP Accept | Client sends `Accept: application/zip` on GET of a folder |

Handy for grabbing a whole album in one file. Protocol note: [webdav-capabilities.md](webdav-capabilities.md#zip--archive-download).

## Browse an existing ZIP

On a `.zip` **file**, use the item menu (**⋯**) → **Browse archive** to peek inside without extracting the whole archive to disk. Password-protected ZIPs may ask for a password. This is different from “download folder as ZIP”.

## Other useful menu actions

Depending on permissions and file type, **⋯** may offer: Open, **Edit** (text), Browse archive, QR, Select, Properties.  
**Download all files** (when shown) downloads **files in the current folder only** — it is not a recursive ZIP of subfolders.

Image paste into the current folder may upload a clipboard image where supported.

## Add another WebDAV server (in the browser)

In the web UI sidebar you can **Add server**: name, WebDAV URL, optional user/password, then switch between:

- **This phone’s server** (the one you opened), and  
- **Other WebDAV endpoints** (another NAS, cloud WebDAV, second phone, …).

Remote URLs are fetched **through this device** (same-origin proxy) so the browser can reach them even when the other server would block cross-origin calls. Loopback / this app’s own address stays direct.

This does **not** make WebDavFS a cloud account — it only lets the browser UI talk to another WebDAV URL while you are already using this phone as a hub.

## Hash routes (when enabled)

With password + admin + options on, the UI may open `#logs` or `#settings`-style views for admin tools. Ordinary sharing does not require these.

## Office Online / WOPI

Optional hooks exist for office “open in browser” flows, but the Android app does **not** ship a ready Office Online discovery URL. Prefer the built-in **text Edit/preview**. Do not expect Word/Excel Online unless you configure an external WOPI host yourself.

## Turn web UI off

Advanced → **Web interface** off if you only want classic WebDAV/FTP clients.
