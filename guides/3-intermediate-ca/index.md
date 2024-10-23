# Managing Intermediate CAs

## Name Constraints

Per B4CKSP4CE CA policy, each intermediate CA should have strict Name Constraints.
It means that every intermediate CA should be able to issue certificates only for a specific subset of resources.

First of all, determine a list of resources that the new CA should be able to issue certificates for.
It can be:

1. Specific domain, e.g. `project.example.com`
2. Wildcard, e.g. `*.example.com`
3. IPv4 range defined as a netmask `10.0.2.0/255.255.255.0` or `10.0.2.2/255.255.255.255`.
4. IPv6 range, e.g. `D91:652E:271A:0:0:0:0:0/FD91:652E:271A:FFFF:FFFF:FFFF:FFFF:FFFF`

Let's say I want to set up a CA for internal SIP telephony service:

1. Devices in `*.sip.bksp.in` domain.
2. Devices IPs in `10.133.7.0/24` subnet.
3. SIP server at `sip.bksp.in` and `sip.bksp.dn42`.

So the Name Constraints for the new CA will be:

1. `permitted;DNS:.sip.bksp.in`
2. `permitted;IP:10.133.7.0/255.255.255.0`
3. `permitted;DNS:sip.bksp.dn42`

Resulting in the following `nameConstraints` attribute:

```
nameConstraints=permitted;DNS:.sip.bksp.in, permitted;IP:10.133.7.0/255.255.255.0, permitted;DNS:sip.bksp.dn42
```

## Creating a new Intermediate CA

### XCA

1. Open XCA database.
2. Connect Yubikey and ensure that PKCS#11 provider is set up.
3. Check PKCS#11 with `Token` -> `Manage Security Token`. You should see contents of your Yubikey.
4. Open `Certificates` tab, click on `New Certificate`.
5. Ensure that `Use this Certificate for Signing` is checked, and `Root CA` is selected.
6. Select `Intermediate CA` as a template, click `Apply all`.
7. On the `Subject` tab, fill the `Internal Name` field with the name of the new CA. Let's call it `SIP S1`.
8. In `Common Name` field, enter `B4CSKP4CE S1`.
9. Click on `Generate a new key`. Set name to `SIP S1`, key type to `EC` and curve to `secp384r1`.
10. Generate a new key by clicking `Create`.
11. Open `Extensions` tab.
12. Set `Time range` field value. You should aim at minimum reasonable value for your case. Max value is 15 years. Click on `Apply`.
13. Adjust `CRL distribution point` field URI. Set it to `https://ca.bksp.in/s1/revoke.crl`.
14. Open `Advanced` tab.
15. Click `Edit` and paste the `nameConstraints` attribute we've built before.
16. Click `OK` to sign the certificate.
17. Yubikey will ask for a PIN. Enter it.

### Repository

1. Create a new directory in `public` folder of this repo. Name it after your CA code, e.g. `s1`.
2. Export PEM-coded certificate to the `public/s1/bksp-s1.pem` file.
3. Sign an initial CRL using XCA's `Revocation List` tab. Set time range to 1 year, click on `Apply`.
4. Export PEM-coded CRL to the `public/s1/revoke.crl` file.
5. In the `public/s1` directory, run `openssl x509 -text -in bksp-s1.pem > bksp-s1.txt` to create a human-readable certificate dump.
6. Add a new entry to the `public/index.md` Intermediate CAs list.
7. Ensure that your Git configuration includes your valid name, email, and trusted signing key.
8. Commit and push changes to the repository.
