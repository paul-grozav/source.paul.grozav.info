---
layout: page
ptitle: iPXE
---

<a href="https://ipxe.org/" target="_blank">ipxe.org</a>:
```bash
# This will generate a floppy image and an .iso image in your current directory:
( cat - <<'EOF'
  apk add git perl make gcc libc-dev xz-dev syslinux xorriso &&
  cd /root &&
  git clone https://github.com/ipxe/ipxe.git &&
  cd ipxe/src &&
  make &&
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
```
Then use 
