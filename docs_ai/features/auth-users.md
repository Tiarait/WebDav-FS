# Auth & users

## Two modes (per server)

| Setting | Behavior |
|---------|----------|
| **Use password** off | Anyone who can reach the address sees that server’s **Folder**. Optional **Read only** in Advanced. |
| **Use password** on | Clients must log in. **Users** replaces the single Folder row. |

On shared Wi‑Fi or anything reachable from the Internet, prefer **password on**.

## Users screen

When password is on:

| Field | Notes |
|-------|--------|
| Login / password | Change the default `admin` / `admin` before exposing the server |
| Role | **Admin**, **User**, or **Guest** |
| Read / Write | What that account may do |
| Folder(s) | Paths that user may see |
| Quota | Optional size limit (GB) |

**Admin** is required for **Show logs in web client** (browser `#logs` / admin log view).

Free: max **2** users. You generally cannot remove the last remaining user.

## Tips

- Read-only anonymous share: password **off** + **Read only**.  
- Different people, different folders: password **on** + Users.  
- Status → Mode shows **Auth** / **RO**.

Related: [../user/guide.md](../user/guide.md), [free-vs-pro.md](free-vs-pro.md), [web-client.md](web-client.md).
