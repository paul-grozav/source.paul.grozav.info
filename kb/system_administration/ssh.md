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

#### SCP limit bandwidth
By default scp uses as much bandwidth as possible. To limit it you can use the `-l` parameter. It's value should be in `KBit/s`. The limit in the second case is set to `256 KBytes/s`:
```bash
paul@me:~$ scp server:/path/my.file ./my.file
paul@me:~$ scp -l $(( 256 * 8 )) server:/path/my.file ./my.file
```

#### SSH Tunnels
Say we have the following command:

```bash
ssh devel -N -L 1234:google.com:80
```

the ssh devel part of the command, simply creates a SSH connection to the devel machine. The `-N` option will prevent ssh from opening a bash on the devel machine, because we don’t need to run commands on that machine, we’ll only create a tunnel.

Something like this:
```txt
==========================================================
                            INTERNET
          __________________   _________________
======== | ================ | | =============== | ========
         |                  | |                 |
         |                  | |                 |
         |                  | |                 |
      _______             ________           _______
     |       |           |        |         | google|
     | local |           | devel  |         | .com  |
     |_______|           |________|         |_______|
```
Now the fun part is `-L 1234:google.com:80`. This option tells ssh to create a connection between my local machine port `1234` and `google.com` port `80`. But it will not be a direct connection between the local machine and the google.com machine, instead all the traffic will pass through the devel machine. Actually the devel machine will act as a gateway, it will only forward packets from google.com to the local machine, and from the local machine to google.com. This way, from google.com’s point of view, the packets come from the devel machine(google will not know about the local machine), while from you’re side you could simply connect to `127.0.0.1` port `1234` and you’ll have google.com answering.

Of course this is not a very useful example but imagine that you are with your friend at his PC and you have you’re computer at home, powered on and you can ssh into it through the internet. In the same LAN (at home) you have another computer that you’d like to access from your remote location.

Something like this:
```txt
==========================================================
                            INTERNET
           ____________________
========= | ================== | =========================
          |                    |
          |                    |
          |                    |
      __________          ____ | ____________________________
     |          |        |     |    LOCAL AREA NETWORK       |
     | FRIEND's |        |     |         ( HOME )            |
     |    PC    |        |   ________         ________       |
     |__________|        |  |   MY   |_______| MOM's  |      |
                         |  |COMPUTER|       |COMPUTER|      |
                         |  |________|       |________|      |
                         |___________________________________|
```
You can’t access your mom’s PC directly(although it’s powered on) because you don’t have a port forwarding in your router for that. You could simply create a tunnel that takes you to 192.168.3.68:8932 (assuming that your mom’s IP is 192.168.3.68 and you would like to connect to her computer on port 8932).

In fact in such a scenario you can bring any HOST:PORT combination that <<MY COMPUTER>> can access, to your local computer ( <<FRIEND’s PC>> ). You could connect to your own computer on a different port using `-L 1234:127.0.0.1:9192`, or to your router at home using `-L 1234:192.168.0.1:22`, etc.

I should also mention that you could create multiple tunnels in one command by adding more than one -L option. For example connect over IPMI to your home server:

```bash
ssh home -N -L 8080:192.168.0.2:80 -L 5900:192.168.0.2:5900
```
This brings IPMI’s `80` port to local `8080` (so that I can log into the administration web interface) and then brings port `5900` to local `5900` (to be used by the java viewer).
