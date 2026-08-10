# Settings field catalog

Maps **what you see in the app** to internal field names (for assistants / maintainers).  
End-user explanations: [../user/guide.md](../user/guide.md).

When the app UI and this table disagree, **trust the app**.

---

## Per-server: Connection (`settingsListLess`)

| UI (concept) | Writes | Notes |
|--------------|--------|-------|
| Port | `port` | Also pushed to live `runtime.serverConfig.userPort` when running |
| Use password | `useUsers` | Runtime: `passEnabled`. When on → Users entry; when off → Folder |
| Users | *(navigation)* | Data via `UtilsPro.readUsers` / `writeUsers` — see [auth-users.md](auth-users.md) |
| Folder | `rootFolder`, often `curUri` / `sdUriMap` | SAF grants may update URI maps |
| IP | `ipType` | `"all"` or a specific host; see [networking.md](networking.md) |

---

## Per-server: SSL (`settingsListSsl`, WebDAV only)

| UI | Writes | Notes |
|----|--------|-------|
| SSL (https) | `useSsl` | Sets protocol `https` / `http` on runtime |
| Certificate | `sslCertificateId` (via cert screens) | Opens certificate library UI |

Legacy SSL fields still exist on `ServerProfile` for migration (`sslCertMode`, paths, ACME email/domain, `cacheCert`, …) but the product path uses the **global certificate library** + `sslCertificateId`. See [ssl.md](ssl.md) and [ssl-certificates-flow.md](ssl-certificates-flow.md).

---

## Per-server: Advanced (`settingsListMore`)

| UI | Writes | Free / Pro |
|----|--------|------------|
| Server type | `serverType` | `"webdav"` / `"ftp"` |
| Buffer size | `bufferSize` | WebDAV; `0` = device default (`effectiveBufferSize`) |
| Web interface | `useWebInterface` | WebDAV |
| Client UI | `clientUi` | `"vue"` / `"modern"` / `"old"` / `"nojs"` when web UI on |
| Custom headers | `customHeadersRaw` | **Pro** |
| CORS | `corsMode` | `"allow_all"` / `"off"` |
| File metadata cache | `fileCacheEnabled` | WebDAV; default **true**; live; Status Mode chip |
| Read only | `permissionWrite` (inverted in UI) | Only when password is off |
| Select hidden files | `UtilsPro.writeHidden` (sidecar file) | **Pro** |
| Hide hidden | `hideHidden` | **Pro** UI gate |
| Show folder size | `showFolderSize` | |
| Report quota in PROPFIND | `reportQuotaInPropfind` | |

---

## Per-server: Autostart & logs (extras)

| UI | Writes |
|----|--------|
| Start with app | `startWithApp` |
| Start with device (boot) | `startWithDevice` |
| Logs enabled | `logsEnabled` |
| Show logs in web client | `logsShowInWebClient` |

Deprecated profile flag: `enabled` (prefer `startWithDevice`).

---

## Per-server: Actions

| UI | Effect |
|----|--------|
| Reset settings | Resets profile fields to defaults (keeps id/name/port as implemented) |
| Delete server | Removes profile (+ data); free/pro list constraints apply when adding |

---

## App-wide (`settingsListApp` → `AppSettings` + actions)

Grouped in UI: **Application** → **Permissions** → **System**.

| UI | Pref / action | Notes |
|----|---------------|--------|
| Theme | `theme_key` | `""` = system; `"light"` / `"dark"` |
| Language | `app_lang` | `""` = system |
| Disable ads (session) | Session flag via `ExtendUtils` | Free only |
| Ad consent | UMP / consent form | Free only |
| Battery optimization | System / OEM screens | Not a pref key |
| Folder / storage permissions | Runtime / all-files access | [../development/permissions-saf.md](../development/permissions-saf.md) |
| Notification permissions | Runtime (API 33+) | Soft reminder on start also exists |
| About | `AboutAppSheet` | Site, email, Play, GitHub (assets icons by flavor) |
| Feedback | mailto dialog | Subject string `feedback_email_subject` |
| Help | Link dialog | |
| Ask AI | DeepWiki link | |
| Reset all data | `ServerFleet.resetAllData()` (+ stop, clear certs/prefs as implemented) | Destructive |

Other `AppSettings` keys commonly used in code:

| Key | API | Role |
|-----|-----|------|
| `c_server_starts` | `countServerStart` | Free ad thresholds |
| `show_exit_b` | `showExitBtn` | Exit button visibility |
| `legacy_server_prefs_migrated` | migration flag | One-shot move into fleet profiles |
| `version` | `prevVersion` | Last-seen app version |

Legacy single-server prefs (`port_key`, `path_key`, `use_ssl_key`, …) are migrated into `ServerProfile` then cleared — do not write new features there.

---

## Status tab (read-only)

Not settings rows — live metrics and Mode chips (Auth, RO, Web, CORS, Cache). See [app-ux.md](app-ux.md) and the [user guide](../user/guide.md#6-status-tab-metrics).

---

## Full `ServerProfile` property list

| Property | Type | Default (as declared) |
|----------|------|------------------------|
| `id` | String | required |
| `name` | String | `"Server"` |
| `enabled` | Boolean | `true` (deprecated) |
| `startWithDevice` | Boolean | `false` |
| `startWithApp` | Boolean | `false` |
| `logsEnabled` | Boolean | `true` |
| `sortOrder` | Int | `0` |
| `port` | Int | `8080` |
| `serverType` | String | `"webdav"` |
| `rootFolder` | String | `""` |
| `curUri` | String | `""` |
| `sdUriMap` | Map | empty |
| `useUsers` | Boolean | `false` |
| `permissionWrite` | Boolean | `true` |
| `ipType` | String | `"all"` |
| `bufferSize` | Int | `0` |
| `useSsl` | Boolean | `false` |
| `sslCertificateId` | String | `""` |
| `sslCertMode` … ACME / keystore legacy fields | various | see source |
| `useWebInterface` | Boolean | `true` |
| `clientUi` | String | `"vue"` |
| `corsMode` | String | `"allow_all"` |
| `fileCacheEnabled` | Boolean | `true` |
| `customHeadersRaw` | String | `""` |
| `hideHidden` | Boolean | `false` |
| `showFolderSize` | Boolean | `false` |
| `reportQuotaInPropfind` | Boolean | `false` |
| `logsShowInWebClient` | Boolean | `false` |
| `passVal` | String | `"admin:admin"` |
| `createdAt` / `updatedAt` | Long | now |

Constants: `DEFAULT_ID = "default"`, `DEBUG_SECOND_ID = "debug2"`.

---

## Free / Pro gates (settings)

| Feature | Gate |
|---------|------|
| Custom headers | Pro |
| Hidden file list / hide hidden | Pro |
| Add server beyond cap | Free max **2** |
| Add user beyond cap | Free max **2** |
| Ads rows in app settings | Free only |
