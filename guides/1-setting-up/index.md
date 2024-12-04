# Setting up

This guide will help you set up tools needed for managing CA.

## Prerequisites

1. We use `XCA` as a GUI tool for managing certificates.
2. Yubikey 5 is our HSM of choice. Install Yubico PKCS#11 provider `libykcs11`
3. If you already have a CA and database, open it in XCA. If not, create a new database.

## Setting up Yubikey in XCA

1. Open existing or create new database.
2. Go to `Settings` -> `PKCS#11 Provider`.
3. Click on `Search`, then select system's lib directory. For macOS & Homebrew, it's `/opt/homebrew/lib/`.
4. After selecting the directory, click on `Search`. You should see `libykcs11` in the list.
5. Check it. You should see a green checkmark next to it.
6. Click on `OK` and close the settings window.

## Importing templates

Open [CA_templates.pem](./CA_templates.pem) in XCA to import presets for B4CKSP4CE CA.
