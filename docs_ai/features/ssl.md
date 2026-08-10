# SSL / HTTPS

WebDAV can serve **HTTP** or **HTTPS**. FTP mode has no HTTPS option in the app.

## Per server

1. Server → **Settings** → turn on **SSL (https)**.  
2. Open **Certificate** and pick (or create) a profile from the shared library.  
3. **Start** or **Restart** so clients see that certificate.

Several servers can reuse the same certificate profile.

## Modes

| Mode | When to use |
|------|-------------|
| **Auto** | Quick local HTTPS (self-signed). Browsers warn until you trust/export the cert |
| **Import** | Your own keystore (PKCS12/JKS) or PEM/CRT + private key |
| **ACME** | Let’s Encrypt for a real domain (DNS TXT challenge) |

Tap-by-tap (regenerate, export PEM, ACME, Status): [ssl-certificates-flow.md](ssl-certificates-flow.md).

## ACME / renewal

Product steps also summarized in the [user guide](../user/guide.md#lets-encrypt-outline). Renewal is **manual** — the app warns before expiry; prepare DNS again and re-issue.

## If clients still see the old cert

Restart the server after changing the selected certificate. If you cannot delete a profile, another server may still be using it.
