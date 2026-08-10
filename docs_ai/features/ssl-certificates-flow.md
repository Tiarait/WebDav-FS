# SSL certificates — where to tap

Modes overview: [ssl.md](ssl.md).

## Idea

Certificates live in a **shared library**. Each WebDAV server with HTTPS **selects** one profile. Several servers can reuse the same one.

## Open the library

Server → **Settings** → turn on **SSL (https)** → **Certificate**.

## Modes when adding / editing

| Mode | When to use |
|------|-------------|
| **Auto** | Quick local HTTPS (self-signed) |
| **Import** | Your own PKCS12/JKS, or **PEM/CRT + private key** |
| **ACME** | Let’s Encrypt for a real domain (DNS-01) |

## Status (any mode)

On the certificate screen, **Status** shows things like subject, issuer, expiry, fingerprint (SHA-256), and whether this profile is the one the server is using. Use it to confirm the cert clients will see after a restart.

## Auto (self-signed)

| Action | What it does |
|--------|----------------|
| **Regenerate** | Creates a new self-signed cert (confirm first). Old trust on PCs becomes invalid — re-export/trust if needed. |
| **Export public certificate (PEM)** | Saves the public cert so you can import/trust it on a computer or other device. |

Browsers will warn “Not private” until they trust this cert — that is expected.

## Import

1. Choose keystore (**PKCS12/JKS**) **or** separate **PEM/CRT + KEY** files.  
2. Enter passwords if the keystore/key requires them.  
3. Save, select the profile on the server, **restart** HTTPS.

## ACME (Let’s Encrypt)

1. Enter **domain** + **email** (and staging only if you are testing).  
2. **Prepare DNS challenge** → create the **TXT** record at your DNS provider (copy/share from the app).  
3. Wait for DNS (often a few minutes) → **Verify and issue**.  
4. Optional: **Share/save DNS record** for renewals; **Export certificate and private key** for backups.  
5. Select the profile on the server and restart if needed.  
6. Connect with `https://your-domain:port`.

Renewal is **manual**. The app warns before expiry — prepare DNS again and re-issue.

## After any change

Start or **Restart** the server so clients pick up the new certificate. If delete is blocked, another server may still be using that profile.

More steps: [../user/guide.md](../user/guide.md#10-how-to-connect).
