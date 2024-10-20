---
layout: page
title: B4CKSP4CE CA
---

# B4CKSP4CE Internal CA

This is the B4CKSP4CE internal Certificate Authority. It is used to sign certificates for internal services and infrastructure.

## Policies

1. Don't trust this CA. Just don't.
2. Wherever you need it, enable this CA only for your particular application. Never install it to system trust stores.
3. This CA uses a secp384r1 ECDSA with SHA256 defaults. RSA-3072 is also supported for selected intermediates.
4. Every intermediate CA should have strict restricted-first Name Constraints.

## Overview

1. Root CA is stored on HSM in RØ team possession.
2. DKEK-encrypted Root CA key is backed up in three locations.
3. HSM DKEK (Device Key Encryption Key) backup is 3-of-5 split between RØ team members.

## Security Contact

Please find a ssh-ed25519 public key and email address in the snippet below.
Expect all replies to be signed with this key.

```sh
# Extract the public key from the Root CA certificate
curl -fSsl https://ca.bksp.in/root/bksp-root-ca.pem | openssl x509 -noout -pubkey > root-ca.pub

# Download the contact proof signature
curl -fSsl https://ca.bksp.in/stuff/noc-contact-proof.asc > noc-contact-proof.asc

# Verify the signature
echo -ne "Security: noc@bksp.in (ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJRwsb2wqvmakJnI9g8LQW5tTQJrgixFci/MTxSIEpq4)" | \
openssl dgst -sha256 -verify root-ca.pub -signature noc-contact-proof.asc -binary -
```

## Root Certificate

- **B4CKSP4CE Root CA**
- **SHA1 Fingerprint**: `DF:C4:90:C7:C8:E9:6C:AB:18:B6:9E:C4:61:FF:A1:CB:1A:34:E9:EE`
- **SHA256 Fingerprint**: `59:18:FB:31:C7:18:06:C7:05:7A:E2:EE:A6:AA:35:32:CC:3B:8B:8E:80:4A:25:9A:E0:6C:EA:84:22:88:C1:18`
- **Not Before**: 19 October 2024 00:00:00 UTC
- **Not After**: 19 October 2049 23:59:59 UTC
- **Revocation List**: [CRL](./root/revoke.crl)
- **Certificate**: [PEM](./root/bksp-root-ca.pem), [TXT](./root/bksp-root-ca.txt)

## Intermediate Certificates

### B4CKSP4CE A1

Intermediate CA for infrastructure services.

- **Signed by**: B4CKSP4CE Root CA
- **SHA1 Fingerprint**: `D8:AA:92:AD:BD:4F:40:5D:B4:97:FB:53:CF:57:13:46:2E:FE:0D:BF`
- **SHA256 Fingerprint**: `01:36:6D:A9:E4:22:A0:71:9A:56:2C:64:DB:75:AD:CC:0C:7B:EC:BC:5C:77:07:8E:92:F7:E2:A3:60:1F:D0:02`
- **Not Before**: 20 October 2024 00:00:00 UTC
- **Not After**: 19 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a1/revoke.crl)
- **Certificate**: [PEM](./a1/bksp-a1.pem), [TXT](./a1/bksp-a1.txt)
