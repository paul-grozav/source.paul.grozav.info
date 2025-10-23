---
layout: page
title: WireGuard
---

```sh
# Generate a new server private key
wg genkey
# Then use that server private key to get it's public part
echo MY_PRIVATE_KEY | wg pubkey
# Remember, that your private key goes into the wg0.conf and the public key goes
# into peer's configuration file, section [Peer], property PublicKey .

# Check peer latest handshake
wg show wg0

wg-quick up wg0
wg-quick down wg0
```
