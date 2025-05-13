---
layout: page
ptitle: iPXE
---

<a href="https://ipxe.org/" target="_blank">ipxe.org</a>:
```bash
# ============================================================================ #
# This will generate a floppy image and an .iso image in your current directory:
( cat - <<'EOF'
  apk add git perl make gcc libc-dev xz-dev syslinux xorriso &&
  cd /root &&
  git clone https://github.com/ipxe/ipxe.git &&
  cd ipxe/src &&

  # nano config/console.h # uncomment CONSOLE_SERIAL if you need it

  # Make all (default config)
  make &&

  # echo -e '#!ipxe'"\necho Starting iPXE embeded script!\nifstat\ndhcp\nshell"\
  #   > /mnt/my_script.ipxe

  # Make .iso
  # make bin/ipxe.iso EMBED=/mnt/my_script.ipxe &&

  # For HTTPS support:
  # apk add openssl openssl-dev &&
  # make bin/ipxe.iso TRUST=/etc/ssl/certs/ca-certificates.crt \
  #   DEBUG=tls,x509:3,certstore,privkey
  # Or with TRUSTing LE's R3:
  # curl https://letsencrypt.org/certs/lets-encrypt-r3.pem -o /root/r3.pem
  # make bin/ipxe.iso TRUST=/root/r3.pem DEBUG=tls,x509:3,certstore,privkey
  # See also: make bin-x86_64-efi/ipxe.efi -j10 \
  #   DEBUG=dhcp,tftp,http EMBED=/mnt/my_script.ipxe

  cp bin/ipxe.dsk /mnt/ &&
  cp bin/ipxe.iso /mnt/ &&
  exit 0
EOF
) | podman run \
  --interactive \
  --rm \
  --name ipxe_builder \
  --volume $(pwd):/mnt:rw \
  docker.io/alpine:3.16.2 \
  /bin/sh
# ============================================================================ #
```
Then boot from it.
