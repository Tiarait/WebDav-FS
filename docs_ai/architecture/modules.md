# Modules

Gradle includes the main **app** plus **Free** / **Pro** flavor libraries and an **ACME** helper for Let’s Encrypt. A separate **web client** front-end is bundled into the APK for the browser file manager.

| Piece | Role for users |
|-------|----------------|
| App | Phone + TV UI, servers, notifications |
| Free / Pro | Ads vs no ads; Pro-only options |
| ACME | Issue trusted certificates when you choose ACME mode |
| Web client | Optional UI in the browser at the server URL |
| Locale scripts | Keep translations in sync (maintainers) |

Build flavors and SDK: [../build/README.md](../build/README.md).  
Web UI for users: [../features/web-client.md](../features/web-client.md).
