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
curl -fSsl https://ca.bksp.in/root/bksp-root.crt | openssl x509 -noout -pubkey > root-ca.pub

# Download the contact proof signature
curl -fSsl https://ca.bksp.in/sig/noc-contact-proof.asc > noc-contact-proof.asc

# Verify the signature
echo -ne "Security: noc@bksp.in (ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJRwsb2wqvmakJnI9g8LQW5tTQJrgixFci/MTxSIEpq4)" | \
openssl dgst -sha256 -verify root-ca.pub -signature noc-contact-proof.asc -binary -
```

## B4CKSP4CE Root CA

- **Serial Number**: `69C682255E615688`
- **SHA1 Fingerprint**: `D8:17:C6:A5:5F:BA:C7:B6:02:59:C5:77:B6:64:58:2E:BC:66:F2:F3`
- **SHA256 Fingerprint**: `77:F3:4B:65:07:1D:2B:14:97:ED:63:A6:14:C6:98:43:2D:48:BD:31:9E:30:96:50:D6:BA:0A:15:1B:CB:AB:7D`
- **Not Before**: 22 October 2024 00:00:00 UTC
- **Not After**: 11 October 2049 23:59:59 UTC
- **Revocation List**: [CRL](./root/revoke.crl)
- **Certificate**: [PEM](./root/bksp-root.crt), [TXT](./root/bksp-root.txt)

### B4CKSP4CE A1

Internal Devices CA.

- **Serial Number**: `4EAA10FC2ED128C4`
- **SHA1 Fingerprint**: `5A:53:50:37:8D:89:1A:BF:5E:7B:51:56:03:E1:15:6B:13:16:BF:73`
- **SHA256 Fingerprint**: `CF:75:F1:09:4F:47:CE:45:BE:5F:5A:FF:F8:56:13:DF:FA:D6:E3:AF:E9:FF:D6:F0:FE:F9:F1:47:FF:DB:34:DD`
- **Not Before**: 23 October 2024 00:00:00 UTC
- **Not After**: 12 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a1/revoke.crl)
- **Certificate**: [PEM](./a1/bksp-a1.crt), [TXT](./a1/bksp-a1.txt)
- **Name Constraints**:
  - **DNS**: `.int.bksp.in`
  - **IP**: `10.0.2.0/23`
  - **IP**: `FD91:652E:271A::/48`

### B4CKSP4CE A2

Internal Services CA.

- **Serial Number**: `69379D58AF837B49`
- **SHA1 Fingerprint**: `AB:E1:7F:97:40:11:9D:D5:5C:3C:7C:BF:44:B6:63:D4:1C:F4:24:F6`
- **SHA256 Fingerprint**: `44:AA:93:25:75:D7:06:78:66:26:6C:D2:47:20:51:CB:BF:AB:EE:1C:49:00:06:0C:86:45:4D:D6:02:64:69:F4`
- **Not Before**: 23 October 2024 00:00:00 UTC
- **Not After**: 22 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a2/revoke.crl)
- **Certificate**: [PEM](./a2/bksp-a2.crt), [TXT](./a2/bksp-a2.txt)
- **Name Constraints**:
  - **DNS**: `.svc.bksp.in`
  - **IP**: `10.0.2.0/23`
  - **IP**: `FD91:652E:271A::/48`

### B4CKSP4CE M1

Testing CA.

- **Serial Number**: `1CAA7CAA29BD87DA`
- **SHA1 Fingerprint**: `8D:62:E6:0F:87:79:F9:47:B4:0C:71:B4:19:65:9D:14:3B:99:BC:D0`
- **SHA256 Fingerprint**: `CF:C5:4A:39:5A:10:9F:F5:12:36:0D:20:76:2A:9D:59:C6:DE:ED:11:1A:8F:99:A1:8C:97:66:F9:C4:7D:9B:DA`
- **Not Before**: 23 October 2024 00:00:00 UTC
- **Not After**: 23 October 2039 23:59:59 UTC
- **Revocation List**: [CRL](./m1/revoke.crl)
- **Certificate**: [PEM](./m1/bksp-m1.crt), [TXT](./m1/bksp-m1.txt)
- **Name Constraints**:
  - **DNS**: `test.ca.bksp.in`
  - **DNS**: `.test.ca.bksp.in`
  - **DNS Prohibited**: `badnc.test.ca.bksp.in`
