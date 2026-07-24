# Networking & discovery

## Address shown in the UI

Built from **`ServerConfig.address`** / **`displayHost`**:

```text
{userProt}://{host}:{userPort}
```

| Piece | Source |
|-------|--------|
| `userProt` | `ftp` / `http` / `https` from profile (`serverType` + `useSsl`) |
| `host` | From `ipType` via `UtilsNetwork.hostForProfile` (LAN IP when `ipType == "all"`) |
| `userPort` | `ServerProfile.port` |

IPv6 hosts are bracketed in the URL. ViewModels expose this as `serverAddress` for share / QR / notification.

Sharing the address uses the system share sheet (`shareText`) on phone UI — not a forced browser open.

---

## `UtilsNetwork`

Path: `utils/UtilsNetwork.kt`

| API | Role |
|-----|------|
| `getAllNetworkAddresses()` | Choices for IP picker: `"all"`, `127.0.0.1`, then up interfaces (IPv4 preferred). Cached ~30s |
| `invalidateCache()` | Called on Wi‑Fi / AP state changes |
| `ipAddressInLocalNetwork` | Best-effort site-local IPv4, else `127.0.0.1` |
| `hostForProfile(ipType, detectedLocalHost)` | Resolve bind/display host for a profile |
| `displayIpType(ipType)` | Human label in settings |
| `isAvailablePort(port)` | Probe with `ServerSocket`; `BindException` → unavailable |

Legacy helper `ipAddressInLocalSettings` still reads app-level IP prefs; **fleet profiles use `ServerProfile.ipType`**.

---

## Bind host (`ipType`)

| `ipType` | Ktor | FTP |
|----------|------|-----|
| `"all"` (default) | Connector host omitted → all interfaces | Bind `0.0.0.0` |
| Specific IP / hostname | Connector `host = userHost` | Bind that host |

UI picker is filled from `getAllNetworkAddresses()`.

---

## Wi‑Fi change / restart

`WifiReceiver` (`utils/WifiReceiver.kt`), registered from `App.registerReceivers()`:

1. Listens to connectivity / Wi‑Fi AP broadcasts.
2. Invalidates address cache.
3. Debounces (~1s) then `ServerFleet.refreshAllHosts(newIp)`.
4. If any server is running → `ServerManager.restartRunningServers(fleet)` and refresh notification.
5. WakeLock is kept across stop→start so the process is not slept mid-restart.

TV may skip some notification refresh paths; host refresh still applies.

---

## Port conflicts

- Two profiles must not share the same port; start fails with availability / in-use errors.
- `Server.tryStart()` retries and surfaces messages such as:
  - `Port {n} is already in use`
  - `Port {n} is not available`
  - `Access denied for port {n}`
- Special case: if the port is busy but probes show **this app** already serving (`/config-js` or realm `WebDAV FS`), start may treat it as already `Started` (toast about already running).

See [../troubleshooting.md](../troubleshooting.md).

---

## LAN vs internet

The app is designed for **local network** access. Exposing a phone to the public internet (port forward, DynDNS, CGNAT) is a user networking concern — see [user/guide.md](../user/guide.md) — not enforced in code beyond optional HTTPS/auth.
