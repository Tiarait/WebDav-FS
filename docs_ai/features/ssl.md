# SSL / HTTPS

WebDAV can serve **HTTP** or **HTTPS**. FTP has no SSL UI in the product path.

A server profile only stores:

- `useSsl` (boolean)
- `sslCertificateId` (reference into the global certificate library)

## Modes (`SslCertificateProfile.mode`)

| Mode | Value | Behavior |
|------|-------|----------|
| Auto | `auto` | Self-signed cert generated on device (`AutoCertificateGenerator`); optional cache/export |
| Import | `custom` | User-provided keystore (PKCS12/JKS) or PEM + private key |
| ACME | `acme` | Let's Encrypt via `:acme` module (DNS-01 is the supported product path) |

## UI

- Toggle SSL in per-server settings (WebDAV).
- Open certificate list / editor: `CertificateListScreen`, `SslCertificateScreen` (+ TV counterparts).

## Storage

Global under app files, roughly:

- `filesDir/certificates/` — metadata / store
- `certificates_data/{id}/` — material per profile

Exact layout and replication notes for implementers may also appear as `docs/SSL_CERTIFICATES.md` in the full source repository. In this tree see [ssl-certificates-flow.md](ssl-certificates-flow.md).

## ACME / DNS providers

Product steps: [user guide — Let’s Encrypt](../user/guide.md#lets-encrypt-outline). An example DNS-01 walkthrough may exist as `docs/ACME_DNS_NIC_UA.md` in the full source repository.

## Applying certs at runtime

`Server.applyProfile(profile, cert)` where `cert` comes from `SslCertificateRepository` when `sslCertificateId` is set. Engine wiring for Ktor lives under `server/ktor/` (SSL configuration helpers).
