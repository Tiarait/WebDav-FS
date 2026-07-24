# SSL certificates — UI & apply flow

Product overview: [ssl.md](ssl.md). UI and apply chain: this page. Optional longer design notes may exist as `docs/SSL_CERTIFICATES.md` in the full source repository.

## Concepts

| Type | Role |
|------|------|
| `SslCertificateProfile` | One cert entry; `mode` = `auto` \| `custom` \| `acme` |
| `SslCertificateStore` | Persist JSON at `filesDir/certificates/profiles.json` |
| `SslCertificateRepository` | In-memory `StateFlow` + CRUD + `serversUsing` / `canDelete` |
| `SslCertificateResolver` | Copy cert fields onto runtime `ServerConfig` |
| `ServerProfile.sslCertificateId` | Reference only (plus `useSsl`) |

---

## UI screens

| Screen | Path | Role |
|--------|------|------|
| List | `ui/compose/CertificateListScreen.kt` | Pick / edit / delete / add for a server |
| Editor | `ui/compose/SslCertificateScreen.kt` | Mode tabs; save; regenerate; ACME DNS; delete |

TV has parallel certificate screens under `ui/tv/`.

### Create

`SslCertificateRepository.create("")` → `fleet.patchProfile { sslCertificateId = created.id }` → open editor.

### Select

Row click → `patchProfile { copy(sslCertificateId = cert.id) }`.

### Delete

- List: `canDelete` if no server references; else “in use” dialog.
- Editor: delete + clear `sslCertificateId` on all servers that pointed at it.

### Apply after save

`certRepository.save` → `refreshServersUsingCert()` → for each running/attached server with that id: `runtime.applyProfile(profile, cert)` → `SslCertificateResolver.applyToServerConfig`.

---

## Modes (UI → disk → Ktor)

```
SslCertificateScreen tabs
  Auto   → mode "auto"
  Import → mode "custom" (+ importFormat keystore|pem)
  ACME   → mode "acme"
        ↓ save / ACME actions
SslCertificateRepository → SslCertificateStore
        ↓ Server.applyProfile
SslCertificateResolver.applyToServerConfig
        ↓ Ktor ServerConfig.resolveKeyStore()
  auto   → AutoCertificateGenerator.ensureAutoCertificate
  custom → CertificateLoader.loadPem | loadKeystore
  acme   → AcmeCertificateStorage.loadKeyStore
            ↑ prepareDns01Challenge / completeDns01AndStore
              → AcmeCertificateIssuer (:acme)
```

### Auto

Regenerate with `forceRegenerate=true` → update stored password / refresh servers.

### Import

Validate via `CertificateLoader` before save; may clear auto cache when leaving Auto.

### ACME (DNS-01 product path)

1. `prepareAcmeDnsChallenge` → TXT record instructions.
2. User publishes DNS.
3. `completeAcmeDnsCertificate` → store under `certificates_data/{id}/ssl/acme/` → `mode=acme`.

HTTP-01 challenge route also exists for Ktor (`RouteAcmeChallenge`) when needed by the issuer.

---

## Migration

`SslCertificateRepository.migrateEmbeddedFromServers` lifts legacy per-profile SSL fields into the global library once.

Helpers under `server/ssl/`: `AutoCertificateGenerator`, `AcmeCertificateStorage`, `CertificateLoader`, …
