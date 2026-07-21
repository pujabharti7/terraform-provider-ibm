# Vulnerability Fix Report

**Date:** 2026-07-21
**Repo:** pujabharti7/terraform-provider-ibm
**Language:** Go (go1.26.5)
**Alert sources:** govulncheck — https://pkg.go.dev/vuln (Go Vulnerability Database)

## Fix Status

| Source | Vuln ID | Severity | Package | Was | Fixed To | Status |
|---|---|---|---|---|---|---|
| govulncheck | GO-2026-5856 | HIGH | `crypto/tls` (stdlib) | go1.26.3 | go1.26.5 | ✅ Fixed — go directive bumped |
| govulncheck | GO-2026-5039 | MEDIUM | `net/textproto` (stdlib) | go1.26.3 | go1.26.4 | ✅ Fixed — go directive bumped |
| govulncheck | GO-2026-5038 | MEDIUM | `mime` (stdlib) | go1.26.3 | go1.26.4 | ✅ Fixed — go directive bumped |
| govulncheck | GO-2026-5037 | MEDIUM | `crypto/x509` (stdlib) | go1.26.3 | go1.26.4 | ✅ Fixed — go directive bumped |
| govulncheck | GO-2026-5018 | HIGH | `golang.org/x/crypto` | v0.51.0 | v0.52.0 | ✅ Fixed — go.mod bumped |
| govulncheck | GO-2026-4550 | HIGH | `github.com/cloudflare/circl` | v1.6.1 | v1.6.3 | ✅ Fixed — go.mod bumped |

## Informational / No Fix Available

| Vuln ID | Package | Fixed In | Notes |
|---|---|---|---|
| GO-2026-5932 | `golang.org/x/crypto/openpgp` | N/A (unmaintained) | Code does not call this package — no action required |

## Changes Made

### `go.mod`
- Bumped `go` directive: `1.25.8` → `1.26.5`  
  Resolves stdlib vulnerabilities: GO-2026-5856, GO-2026-5039, GO-2026-5038, GO-2026-5037
- Upgraded `golang.org/x/crypto`: `v0.51.0` → `v0.52.0`  
  Resolves: GO-2026-5018 (RSA/DSA DoS in crypto/ssh)
- Upgraded `github.com/cloudflare/circl`: `v1.6.1` → `v1.6.3`  
  Resolves: GO-2026-4550 (incorrect secp384r1 CombinedMult calculation)

### `go.sum`
- Updated automatically by `go mod tidy`

## Vulnerability Details

### GO-2026-5856 — ECH Privacy Leak in crypto/tls (HIGH)
- **Description:** Encrypted Client Hello (ECH) privacy leak when handling certain TLS handshakes.
- **Fix:** Go toolchain upgrade to ≥1.26.5

### GO-2026-5018 — DoS in golang.org/x/crypto/ssh (HIGH)
- **Description:** Pathological RSA/DSA parameters may cause denial-of-service when parsing SSH keys.
- **Affected code:** `resource_ibm_is_ssh_key.go` (parseKey), `data_source_ibm_is_bare_metal_server_initialization.go`
- **Fix:** `golang.org/x/crypto@v0.52.0`

### GO-2026-4550 — Incorrect secp384r1 CombinedMult in cloudflare/circl (HIGH)
- **Description:** Incorrect elliptic curve calculation in CombinedMult for secp384r1 curve.
- **Fix:** `github.com/cloudflare/circl@v1.6.3`

### GO-2026-5039 — Error injection in net/textproto (MEDIUM)
- **Description:** Arbitrary input included in errors without escaping in ReadMIMEHeader.
- **Fix:** Go toolchain upgrade to ≥1.26.4

### GO-2026-5038 — Quadratic complexity in mime.WordDecoder (MEDIUM)
- **Description:** Quadratic time complexity in WordDecoder.DecodeHeader enabling DoS.
- **Fix:** Go toolchain upgrade to ≥1.26.4

### GO-2026-5037 — DoS in crypto/x509 hostname parsing (MEDIUM)
- **Description:** Inefficient candidate hostname parsing enabling DoS.
- **Fix:** Go toolchain upgrade to ≥1.26.4

## Validation

```
govulncheck ./... → No vulnerabilities found (0 actively-called)
go build ./...    → SUCCESS (clean build, no errors)
```

## SLA

| Vuln ID | Severity | Status |
|---|---|---|
| GO-2026-5856 | HIGH | ✅ Fixed — PR raised 2026-07-21 |
| GO-2026-5018 | HIGH | ✅ Fixed — PR raised 2026-07-21 |
| GO-2026-4550 | HIGH | ✅ Fixed — PR raised 2026-07-21 |
| GO-2026-5039 | MEDIUM | ✅ Fixed — PR raised 2026-07-21 |
| GO-2026-5038 | MEDIUM | ✅ Fixed — PR raised 2026-07-21 |
| GO-2026-5037 | MEDIUM | ✅ Fixed — PR raised 2026-07-21 |
