# B4CKSP4CE Certification Authority

This is the B4CKSP4CE internal Certificate Authority. It is used to sign certificates for internal services and infrastructure.

## Installation

```sh
# Create a directory within ca-certificates, then download the root certificate
sudo mkdir -p /usr/share/ca-certificates/bksp
curl -fSsl https://ca.bksp.in/root/bksp-root.crt | sudo tee /usr/share/ca-certificates/bksp/B4CKSP4CE_Root_CA.crt

# Create a symbolic link in system trust store, then add the entry for ca-certificates configuration
echo "bksp/B4CKSP4CE_Root_CA.crt" | sudo tee -a /etc/ca-certificates.conf

# Update the system trust store
sudo update-ca-certificates
```

## Rules of Engagement

1. Don't trust this CA. Just don't.
2. Wherever you need it, enable this CA only for your particular application. Never install it to system trust stores.
3. This CA uses a secp384r1 ECDSA with SHA256 defaults. RSA-3072 is also supported for selected intermediates.
4. Every intermediate CA have strict restricted-first Name Constraints.

## Overview

- Root CA is stored on HSM in RØ team possession.
- Root CA is backed up on two encrypted offline devices, one offsite and one onsite.
- Backup key is divided into 5 shares using Shamir's Secret Sharing Scheme. Each share holded by a different resident.
- More stuff is available in [Guides](./guides/) section.

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

- **Serial Number**: `7B2F15A7C459B07D`
- **SHA1 Fingerprint**: `C0:B8:B4:37:E5:58:C2:33:95:9E:2D:A7:61:43:A5:67:FE:51:A3:C4`
- **SHA256 Fingerprint**: `1A:9F:4B:A1:BC:B5:F5:06:5D:54:57:3C:DA:CE:B6:B9:8C:63:F1:C5:7F:76:C2:09:A5:B3:9D:58:7E:08:B4:AF`
- **Not Before**: 15 November 2024 00:00:00 UTC
- **Not After**: 15 November 2049 23:59:59 UTC
- **Revocation List**: [CRL](./root/revoke.crl)
- **Certificate**: [PEM](./root/bksp-root.crt), [TXT](./root/bksp-root.txt)

### B4CKSP4CE A1

Internal Devices CA.

- **Serial Number**: `34A52889EFA022F8`
- **SHA1 Fingerprint**: `9D:89:CD:93:C2:50:90:55:C3:E5:9C:B5:5D:7E:CC:7D:7E:4C:71:BB`
- **SHA256 Fingerprint**: `F0:88:CF:C8:8D:E0:89:55:D2:83:17:A2:D5:B4:E1:68:37:EC:33:FA:B7:F3:3A:0D:AA:6F:96:DD:DE:DD:1D:6F`
- **Not Before**: 15 November 2024 00:00:00 UTC
- **Not After**: 15 November 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a1/revoke.crl)
- **Certificate**: [PEM](./a1/bksp-a1.crt), [TXT](./a1/bksp-a1.txt)
- **Name Constraints**:
  - **DNS**: `.int.bksp.in`

### B4CKSP4CE A2

Internal Services CA.

- **Serial Number**: `6B47B45F6DB6EC8D`
- **SHA1 Fingerprint**: `4B:79:3F:B4:A0:C4:84:3E:9A:69:49:B1:E1:2C:55:D1:17:2D:A4:F2`
- **SHA256 Fingerprint**: `87:2B:BF:D9:5D:9B:B9:12:16:8A:DF:B9:E4:0B:81:32:08:39:96:42:11:A5:9E:60:0C:91:95:FD:91:FE:A5:3B`
- **Not Before**: 15 November 2024 00:00:00 UTC
- **Not After**: 15 November 2039 23:59:59 UTC
- **Revocation List**: [CRL](./a2/revoke.crl)
- **Certificate**: [PEM](./a2/bksp-a2.crt), [TXT](./a2/bksp-a2.txt)
- **Name Constraints**:
  - **DNS**: `.svc.bksp.in`
