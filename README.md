This is the B4CKSP4CE internal Certificate Authority. It is used to sign certificates for internal services and infrastructure.

## Policies

1. Don't trust this CA. Just don't.
2. Wherever you need it, enable this CA only for your particular application. Never install it to system trust stores.
3. This CA uses a secp384r1 ECDSA with SHA256 defaults. RSA-3072 is also supported for selected intermediates.
4. Every intermediate CA have strict restricted-first Name Constraints.

## Overview

1. Root CA is stored on HSM in RØ team possession.
2. Root CA is backed up on two encrypted offline devices, one offsite and one onsite.
3. Backup key is divided into 5 shares using Shamir's Secret Sharing Scheme. Each share holded by a different resident.

## Security Contact

Please find a ssh-ed25519 public key and email address in the snippet below.
Expect all replies to be signed with this key. You may encrypt your message using this key with [age](https://age-encryption.org/).

```sh
# Extract the public key from the Root CA certificate
curl -fSsl https://ca.bksp.in/root/bksp-root-ca.pem | openssl x509 -noout -pubkey > root-ca.pub

# Download the contact proof signature
curl -fSsl https://ca.bksp.in/sig/noc-contact-proof.asc > noc-contact-proof.asc

# Verify the signature
echo -ne "Security: noc@bksp.in (ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJRwsb2wqvmakJnI9g8LQW5tTQJrgixFci/MTxSIEpq4)" | \
openssl dgst -sha256 -verify root-ca.pub -signature noc-contact-proof.asc -binary -
```

## B4CKSP4CE Root CA

- **Serial Number**: `3CA292E6A9833433`
- **SHA1 Fingerprint**: `B5:C7:87:89:BF:01:58:36:6A:E9:6C:F0:0E:FE:96:66:F2:B9:1B:65`
- **SHA256 Fingerprint**: `65:37:E8:11:4E:BD:ED:77:D8:89:F8:95:7D:1E:54:AC:CC:E1:CE:26:E3:A0:DF:04:3A:40:D3:A1:D2:AA:6E:35`
- **Not Before**: 20 October 2024 00:00:00 UTC
- **Not After**: 19 October 2049 23:59:59 UTC
- **Revocation List**: [CRL](./root/revoke.crl)
- **Certificate**: [PEM](./root/bksp-root-ca.pem), [TXT](./root/bksp-root-ca.txt)

### B4CKSP4CE A1

Intermediate CA for infrastructure Services.

- **Serial Number**: `16632377EC648A4F`
- **SHA1 Fingerprint**: `65:DC:74:93:74:96:53:A8:C9:47:C2:70:2B:E0:56:05:0C:C3:FE:92`
- **SHA256 Fingerprint**: `B9:DE:36:A8:8E:6A:AF:32:BD:C3:81:91:ED:65:0C:FF:D3:6A:43:88:70:FE:2F:6C:D5:4F:2C:62:32:54:6F:E5`
- **Not Before**: 20 October 2024 00:00:00 UTC
- **Not After**: 19 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a1/revoke.crl)
- **Certificate**: [PEM](./a1/bksp-a1.pem), [TXT](./a1/bksp-a1.txt)
- **Name Constraints**:
  - **DNS**: `.svc.bksp.in`
  - **DNS**: `.svc.0x08.in`
  - **IP**: `10.0.2.0/23`
  - **IP**: `FD91:652E:271A::/48`

### B4CKSP4CE A2

Intermediate CA for internal Devices.

- **Serial Number**: `781257F3BBF281E2`
- **SHA1 Fingerprint**: `D2:FA:5A:34:65:FF:DE:65:DE:28:BF:99:76:EE:2C:64:3F:50:4A:6F`
- **SHA256 Fingerprint**: `89:5D:98:C6:C0:2F:BA:0A:1E:9C:00:DF:8B:EE:E5:B8:42:08:43:C8:0A:F5:2C:AF:AE:22:AC:5B:C8:36:37:CB`
- **Not Before**: 22 October 2024 00:00:00 UTC
- **Not After**: 21 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a2/revoke.crl)
- **Certificate**: [PEM](./a2/bksp-a2.pem), [TXT](./a2/bksp-a2.txt)
- **Name Constraints**:
  - **DNS**: `.int.bksp.in`
  - **IP**: `10.0.2.0/23`
  - **IP**: `FD91:652E:271A::/48`

### B4CKSP4CE M1

Intermediate CA for Testing.

- **Serial Number**: `37172756DF4C9AA8`
- **SHA1 Fingerprint**: `76:82:86:66:8D:9F:F3:B1:97:1D:15:2E:BB:55:7E:2E:06:65:DE:40`
- **SHA256 Fingerprint**: `A4:38:55:F3:52:D2:52:D5:CE:BC:7F:E2:C7:99:33:E4:8B:CA:50:C8:B0:FC:42:16:B3:B0:95:B3:5E:10:72:50`
- **Not Before**: 22 October 2024 00:00:00 UTC
- **Not After**: 21 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./m1/revoke.crl)
- **Certificate**: [PEM](./m1/bksp-m1.pem), [TXT](./m1/bksp-m1.txt)
- **Name Constraints**:
  - **DNS**: `.test.ca.bksp.in`
