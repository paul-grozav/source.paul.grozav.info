---
layout: page
ptitle: SSH
---

#### This will generate a public key: `./new_key.pub` and a private one: `./new_key`
```bash
ssh-keygen -b 2048 -t rsa -N "" -q -f ./new_key
```

#### SSH Key PPK to PEM
```bash
paul@me:~$ docker run -it --rm --name="test" debian
root@05673bbb322b:/# apt-get update && apt-get install -y putty
paul@me:~$ docker cp my-key.ppk test:/
root@05673bbb322b:/# puttygen /my-key.ppk -O private-openssh -o /my-key.pem
```

