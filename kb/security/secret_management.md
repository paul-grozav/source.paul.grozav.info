---
layout: page
title: Secret management
---

---

[HashiCorp Vault](https://www.vaultproject.io/) or it's Open Source alternative
[OpenBao](https://openbao.org/), just like
[AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) are designed to
store secrets like passwords and tokens. And it is a very interesting adventure
going down the road to understand how to use these secrets in an application,
especially if you don't want to protect another secret required to authenticate
into OpenBao itself, to get the password you need. The best solution I found to
this problem, so far, is to use it in conjunction with an infrasturcture like
Kubernetes. K8s being the infrastructure that starts and runs your application,
it is capable of injecting a JWT into the application. Then the application
authenticates into OpenBao with that token. In turn, OpenBao verifies the
validity of the token with K8s itself, who emitted it.
<br/><br/>
However, if the secret you are trying to store is an encryption key, then you
shouldn't use the same set of technologies. In this case, what you would like to
use is a Key Management Service (KMS). However, a KMS software will, usually,
not give you the key to decrypt your data. Instead, you should be sending the
data to the KMS service, and it will decrypt it and return the decoded data. So,
for example your data could be encrypted with a data-key, that is stored
encrypted next to the data. When you want to read your data, you send the
data-key (a small file) to the KMS to be decrypted, and then use the decrypted
data-key to decrypt your actual data.
<br/><br/>

# TPM (Trusted Platform Module)
If your server has a TPM and it is enabled, reachable by the Linux OS, you
should see it in `ls -la /dev/tpm*` . To get to interact with it you will need
the package `dnf install tpm2-tools` . You can test it works fine with `tpm2_getcap --version`

## Create the TPM keys
### Create the primary key
`tpm2_createprimary -C o -g sha256 -G ecc -c primary.ctx`

Creates a primary key inside the TPM's owner hierarchy (`-C o`), using `sha256`
for name/hierarchy hashing and an `ecc` (elliptic curve) key type, and writes
the transient handle context to `primary.ctx`. Think of this as the root anchor
inside the TPM for your application's keys.

### Create the child key
`tpm2_create -C primary.ctx -g sha256 -G rsa2048 -u key.pub -r key.priv`

Generates a child 2048-bit RSA encryption key (`rsa2048:rsaes`) under
`primary.ctx` using `sha256`. It outputs the public part to `key.pub` and the
**TPM-sealed** private key blob to `key.priv` (the private key material is
encrypted under the primary key, not exposed as raw plaintext). So the private
key can only be obtained by decrypting it with the TPM's encryption key, that
never leaves the TPM.

### Load key into TPM for subsequent calls
`tpm2_load -C primary.ctx -u key.pub -r key.priv -c key.ctx`

Loads `key.pub` and `key.priv` into the TPM under `primary.ctx` and saves a
usable transient object context to `key.ctx` so subsequent `tpm2_*` commands can reference that key.

## Generate a symmetric application key

### Generate the application symmettric key
`openssl rand -out aes.key 32`

Generates `32` cryptographically random bytes (256 bits) and saves them to
`aes.key` . This random byte string is your symmetric data key for encrypting your file.

### Encrypting the application key using the TPM key
`tpm2_rsaencrypt -c key.ctx -o aes.key.enc aes.key`

Sends `aes.key` (32 bytes, well within RSA-2048 RSAES limits) to the TPM using
`key.ctx` to encrypt it with the TPM key, writing the encrypted key blob to
`aes.key.enc` .

### Remove the unencrypted application key
`rm -f aes.key`

Deletes the unencrypted `aes.key` file from disk so the plaintext symmetric key isn't left behind.

## Encrypting and decrypting your files
`openssl enc -aes-256-cbc -pbkdf2 -salt -in secret.txt -out secret.txt.enc -pass file:<(tpm2_rsadecrypt -c key.ctx -o - aes.key.enc | od -An -tx1 -v | tr -d ' \n'; printf '\n')`

Encrypts `secret.txt` into `secret.txt.enc` .

Then decrypt `secret.txt.enc` to stdout using:
`( set -o pipefail; openssl enc -d -aes-256-cbc -pbkdf2 -in secret.txt.enc -pass file:<(tpm2_rsadecrypt -c key.ctx -o - aes.key.enc | od -An -tx1 -v | tr -d ' \n'; printf '\n') )`

# OpenBao

## Installing OpenBao
I installed the helm chart: https://openbao.org/docs/platform/k8s/helm/ .
and configured it with this initial `values.yaml`:
```yaml
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# Link to the values.yaml:
# https://github.com/openbao/openbao-helm/blob/main/charts/openbao/values.yaml
# ============================================================================ #
server:
  ha:
    enabled: true
    raft:
      enabled: true
      config: |
        cluster_name = "openbao-raft"
        storage "raft" {
          path = "/openbao/data"
        }
        listener "tcp" {
          tls_disable = 1
          address = "[::]:8200"
          cluster_address = "[::]:8201"
        }
        service_registration "kubernetes" {}
```

## Initialize OpenBao
Then we initialize the OpenBao cluster using:
```sh
$ kubectl -n openbao exec -it openbao-0 -- bao operator init
```
This will return the unseal keys and the initial root token:
- Unseal Key 1: AAAA...BBBB
- Unseal Key 2: BBBB...CCCC
- Unseal Key 3: CCCC...DDDD
- Unseal Key 4: DDDD...EEEE
- Unseal Key 5: EEEE...FFFF

- Initial Root Token: SECRETVALUE

Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated root key. Without at least 3 keys to
reconstruct the root key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum
of existing unseal keys shares. See "bao operator rotate-keys" for more
information.

All secrets rest on the persistent storage/disk encrypted. The moment vault
is started from scratch(after a quorrum loss), it does not have access to
the secrets, so it can not serve the clients. That is what we call that vault
starts as being "sealed". Secrets are shared with what is called the "unseal
key". However, because this unseal key is so important, it was split using
[Shamir's algorithm](https://en.wikipedia.org/wiki/Shamir%27s_secret_sharing)
into 5 different keys. You need 3 of those 5 to put together to form the
original unseal key.

The root key is something that a client can authenticate with into the Valut
server, to manage the secrets. To authenticate, the vault needs to be unsealed
first. The root user has full access to all secrets.

## Unsealing OpenBao
To unseal OpenBao you need to run the unseal command with 3 different unseal
keys, on the first pod:
`kubectl -n openbao exec -it openbao-0 -- bao operator unseal AAAA...BBBB`
Then repeat same command with unseal keys `BBBB...CCCC` and `CCCC...DDDD` .

You should now see it is: `Sealed: false`. And `openbao-0` will become healthy,
thus `openbao-1` gets created, but it is sealed again, you can check with `bao
status`. But even worst than that, it is `Initialized: false`. So we first need
to join it into `openbao-0` with command: `bao operator raft join
http://openbao-0.openbao-internal:8200`. Then repeat the same unseal process
again. This fixes `openbao-1`, making it healthy and starting `openbao-2`. Which
needs the same join and unseal procedure to be healthy.

## Management
Login into the vault server by running: `bao login` into one of the pods like
`openbao-0`. You will have to authenticate using the root token.


See also:
- Introduction: https://www.youtube.com/watch?v=ae72pKpXe-s
- Vault in K8s:
  https://www.youtube.com/playlist?list=PLHq1uqvAteVtq-NRX3yd1ziA_wJSBu3Oj