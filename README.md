# HostCrust release signing key

This repository publishes the **public** GPG key HostCrust uses to sign its software
releases — currently the server agent installer (`install.sh` / `uninstall.sh`) served
from `app.hostcrust.com`.

Publishing the key here on github.com, and on <https://hostcrust.com>, gives you a source
**independent of `app.hostcrust.com`**. A signature you verified with a key fetched from
the same origin that served the script would prove nothing if that origin were compromised.

## The key

- File: [`hostcrust-release.pub.asc`](hostcrust-release.pub.asc)
- Type: ed25519, sign-only
- User ID: `HostCrust Release Signing <release@hostcrust.com>`
- **Fingerprint:** `0520 D02C 4ED0 0C39 9790  C36C 85A4 3BD7 73E0 8F47`
- Expires: 2028-11-20

Always check the fingerprint above before trusting an imported copy. The private signing
key is not on any server — it stays in the owner's local keyring — so a compromise of the
app origin cannot forge a valid signature.

## Verify the agent installer

```sh
# Import the key once, from GitHub or hostcrust.com — never only from app.hostcrust.com:
curl -fsSL https://raw.githubusercontent.com/HostCrust/release-keys/main/hostcrust-release.pub.asc | gpg --import

# Then, on each install or upgrade:
curl -fsSL https://app.hostcrust.com/agent/install.sh     -o install.sh
curl -fsSL https://app.hostcrust.com/agent/install.sh.asc -o install.sh.asc
gpg --verify install.sh.asc install.sh   # must print a good signature from the HostCrust release key
less install.sh && sudo sh install.sh    # only after the signature checks out
```

A good signature ties the script — and the agent SHA-256 pinned inside it — to this key.
If `gpg --verify` does not report a good signature from the HostCrust release key, **do not
run the script**: the download was tampered with or served from a compromised origin.

## Mirrors of this key

- <https://hostcrust.com/hostcrust-release.pub.asc>
- <https://github.com/HostCrust/release-keys> (this repository)
