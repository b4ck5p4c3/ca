## XCA

1. Install `xca` and `opensc` packages.
2. If you are on Apple Silicon macOS, install Rosetta and run xca with `arch -x86_64 xca`.
   On other platforms, or x86_64 macOS, you can run `xca` directly.
3. Open an existing database or create a new one.
4. Open `Preferences` -> `PKCS#11 provider` -> `Search`.

### Setting up PKCS#11 provider for macOS

1. Select `stuff/sc-hsm-embedded` in the root of the repository as Directory to search.
2. Click "Search", then check the box next to `sc-hsm-pkcs11.dylib` and click "OK".

## Generate DKEK shares

```sh
sc-hsm-tool --create-dkek-share exports/dkek.pbe --pwd-shares-threshold 3 --pwd-shares-total 5
```

## Init HSM-A

```sh
python3 ./pico-hsm/tools/pico-hsm-tool.py initialize
sc-hsm-tool --initialize --so-pin 3537363231383830 --pin 648219 --dkek-shares 1 --label HSM-A
sc-hsm-tool --import-dkek-share exports/dkek.pbe --pwd-shares-total 3
```

## Generate Root CA key

1. In xca, open "Private Keys" tab, then click "New Key".
2. In Key Type, select "EC Key" on your HSM, the select "secp384r1" as curve.
3. Click "Create".

## Export DKEK-encrypted wrapped Root CA key

```sh
sc-hsm-tool --wrap-key exports/root-ca-key-wrap.bin --key-reference 1 --pin 648219
```

Store the `exports/dkek.pbe` and `exports/root-ca-key-wrap.bin` in a safe place.
You will need them to restore the Root CA key, along with the DKEK shares.

## Import templates

We have prepared a set of [templates for you](./CA_templates.pem) to import into XCA database.
