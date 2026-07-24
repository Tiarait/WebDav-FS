# Auth & users

## Modes

Controlled by **`ServerProfile.useUsers`** (UI: “use password”).

| Mode | Behavior |
|------|----------|
| `useUsers == false` | Anonymous access to profile `rootFolder`. Def user name `"anonymous"`, empty password (`toDefUser`). Global read-only via inverted `permissionWrite` when shown |
| `useUsers == true` | HTTP Basic (WebDAV) / FTP auth against the users file. UI shows **Users** instead of a single folder row |

Default seed credentials string on the profile: `passVal` (e.g. `"admin:admin"`) used when creating the default user entry.

---

## `UserInfo`

Path: `server/data/UserInfo.kt` (kotlinx.serialization).

Typical fields:

| Field | Role |
|-------|------|
| `name` / `pass` | Credentials |
| `permissions` | e.g. `rw` — helpers `canRead` / `canWrite` |
| `rootFolders` | List of `(label, path)` roots for that user |
| `type` | ADMIN / USER / GUEST |
| `maxDirectorySize` | Quota (`-1` = unlimited) |

---

## Storage

| Item | Location |
|------|----------|
| Users file | `filesDir/servers_data/{serverId}/users` |
| Format | JSON array of `UserInfo` |
| API | `UtilsPro.readUsers` / `writeUsers` → `ExtendUtils` file I/O (free and pro) |

Legacy: old `filesDir/users` migrates to the default server id.

Empty or corrupt file → seed from `profile.toDefUser()` / `AppSettings.getDefUser()`.

---

## Free limit

- `UtilsPro.FREE_MAX_USERS = **2**`
- Enforced in **Users UI** (phone `UsersManagementScreen`, TV `TvUsersPanel`): block add + Pro upsell (`R.string.user_free_limit`).
- Some older locale strings may still say “1 user”; prefer the constant and English source string.

---

## Auth wiring (WebDAV)

- When `passEnabled` / `useUsers`: Ktor authenticates via WebDAV Basic auth helpers (`WebDavAuth` / authenticate in `KtorServer`).
- Custom headers / CORS are separate from auth (see settings catalog).

FTP uses the Apache FtpServer user manager populated from the same user model (see [ftp.md](ftp.md)).

---

## UI entry points

| Surface | Types |
|---------|--------|
| Phone | Users management screen / dialogs from settings |
| TV | `TvUsersPanel`, `TvUserEditScreen` |
| Settings row | Shown only when `useUsers` is true |
