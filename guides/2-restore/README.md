# Restoring Root CA

This guide describes how to restore a Root CA key in case if HSM is lost or damaged.

## Encrypted Backup

The Root CA key is backed up in a PFX file. The password for the PFX backup is shared between RØ team members using Shamir's Secret Sharing Scheme. There are 5 shares, 3 of which are required to reconstruct the password.

### Steps

1. Install `ssss` via `brew install ssss` or `apt install ssss`, etc.
2. Find at least three residents holding the shares.
3. Reconstruct the password using `ssss-combine -t 3`. Result is a 16-character alphanumeric password.
4. Decrypt the PFX file using the password.

### Shares

Each share is a small piece of paper with a long string of characters and DataMatrix code.
Share is sealed in a non-transparent plastic envelope. To reveal the share, envelope must be destroyed.

### Shareholders

RØ team members should know who holds the shares.
