---
layout: page
ptitle: The GNU Privacy Guard
---

---

[project website](https://www.gnupg.org/)

[wikipedia](https://ro.wikipedia.org/wiki/GNU_Privacy_Guard)

# Key management
## Generate
```sh
$ gpg --help
# Interactive key generation
$ gpg --full-generate-key
gpg (GnuPG) 2.4.4; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 9
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Tancredi-Paul Grozav
Email address: paul@grozav.info
Comment: Test key
You selected this USER-ID:
    "Tancredi-Paul Grozav (Test key) <paul@grozav.info>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /home/paul/.gnupg/trustdb.gpg: trustdb created
gpg: directory '/home/paul/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/paul/.gnupg/openpgp-revocs.d/3B93FB7EFCF7788270BD9A8485A6CD77EAE8C40F.rev'
public and secret key created and signed.

pub   ed25519 2025-06-03 [SC]
      3B93FB7EFCF7788270BD9A8485A6CD77EAE8C40F
uid                      Tancredi-Paul Grozav (Test key) <paul@grozav.info>
sub   cv25519 2025-06-03 [E]
```
```sh
# non-interactive key gen
gpg --batch --generate-key <<EOF
%echo Generating ECC (Curve25519) key
Key-Type: eddsa
Key-Curve: ed25519
Subkey-Type: ecdh
Subkey-Curve: cv25519
Name-Real: Tancredi-Paul Grozav
Name-Comment: personal key
Name-Email: paul@grozav.info
Expire-Date: 0
Passphrase: $0w3$3)R37
%commit
%echo Done
EOF
```
## Show keys
```sh
# Show existing public and secret keys:
$ gpg --list-secret-keys
$ gpg --list-keys

# Show public key by ID
$ gpg --armor --export 3B93FB7EFCF7788270BD9A8485A6CD77EAE8C40F
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEaD6y+xYJKwYBBAHaRw8BAQdAjwEEC4j5iVJYcrvyhs4DukHqh5mipqbeNIPi
YRqLZs60MlRhbmNyZWRpLVBhdWwgR3JvemF2IChUZXN0IGtleSkgPHBhdWxAZ3Jv
emF2LmluZm8+iJMEExYKADsWIQQ7k/t+/Pd4gnC9moSFps136ujEDwUCaD6y+wIb
AwULCQgHAgIiAgYVCgkICwIEFgIDAQIeBwIXgAAKCRCFps136ujEDyA+AP0dx8To
RbuB1A0OvICwmKmuPrkZIYQQq8CLIwxG4e0qmAEA9z6T5fTpLpgtMdGvVZWYoBpA
ZZANT8G133Hf6Qq/hQ24OARoPrL7EgorBgEEAZdVAQUBAQdA+qk4MyCPK7tmjRbI
5Ys+TKmNWXv0Vhel6ZQ4nebvARUDAQgHiHgEGBYKACAWIQQ7k/t+/Pd4gnC9moSF
ps136ujEDwUCaD6y+wIbDAAKCRCFps136ujED4fwAP9s9cdOcHm2SiPeWb2ZkE+K
/JHCuPbEg2ZQFGEEL42Y6AEAqnjjFZBrGHa2TWeB+Gpqt5LKdbQPyqSKsovgGdMq
DQI=
=Y5hq
-----END PGP PUBLIC KEY BLOCK-----

# Show private(secret) key by ID
$ gpg --armor --export-secret-keys 3B93FB7EFCF7788270BD9A8485A6CD77EAE8C40F
-----BEGIN PGP PRIVATE KEY BLOCK-----

...SECRETDATA...
-----END PGP PRIVATE KEY BLOCK-----
```
## Import key
```sh
gpg --import private-key.asc
```
## Delete key
```sh
# Delete private(secret) key first
gpg --batch --yes --delete-secret-keys AE4650E98DBAB3D6FA7B967B866E788E015D11BA
# Delete public key afterwards
gpg --batch --yes --delete-keys AE4650E98DBAB3D6FA7B967B866E788E015D11BA

```
You might want to add your public GPG key to GitHub/GitLab.

# Using the key

```sh
# Configure git client to sign commits with this key
git config --global user.signingkey 3B93FB7EFCF7788270BD9A8485A6CD77EAE8C40F
# Then add -S (--gpg-sign) to sign the commit. Without configurin git's default
# signing key, you can use --gpg-sign=3B93FB7EFCF7788270BD9A8485A6CD77EAE8C40F
# to sign with a given key id.
git commit -S -m "My commit message"
# Or just configure git to sign all commits by default, automatically, without
# having to pass a sign parameter to the commit command
git config --global commit.gpgSign true
# Confirm commit has valid signature
git log --show-signature
```
