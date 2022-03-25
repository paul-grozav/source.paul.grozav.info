---
layout: page
ptitle: Containers - Podman or Docker
---

### Containers - podman (or docker)

Containers are a way to run processes in isolation. They are much like a jail / chroot in UNIX systems. Though they feel a lot like virtual machines (VMs) they are very much different because they do not rely on a virtualized hardware, and do not run another operating system (kernel).

They just provide a small/minimal set of files (filesystem) that (re)creates the feeling that you are running on a particular Operating System (OS).

Containers are available on both Linux, Windows - though I'm mostly going to describe them on Linux.

Since there is not other OS running in containers, you can't run windows apps/containers on linux and vice-versa. The apps that you are running in isolation (in containers) have their system calls forwarded to the host operating system (the only OS).

So even though you can run debian, centos, alpine, (...) containers that give you the feeling that you have a VM with a different GNU/Linux distribution - it's just a fake feeling.

"Container Images" - are a set of files that provide the minimal filesystem(commands, config files, other files, ...) required to run your apps.

"Containers" - are an isolated environment where the processes run - it feels a lot like a VM, as I said. A container is based on a container image - which provides the filesystem required to run your apps.

You can think of images like a GNU/Linux .iso which you can use to install the same GNU/Linux on multiple machines/computers. And you can think of containers like the machines that were installed using that .iso. So, whenever you start a new container from an image, the filesystem is restored to the original contents of that image.

You can also think of it like OOP - Images are classes and containers are objects/instances. You can create multiple instances(containers) of the same image(class). Even better, you can create new images by extending existing images - just like classes.

#### 1. General

I'm mostly going to describe commands based on `podman`, which I preffer because it's rootless - but `docker` has the same interface, so you should be fine if you replace `podman` with `docker` and keep the rest of the command.

```bash
# Run debian container. -it will connect me to the console of the container and
# --rm=true will make sure that the container is removed once I exit the shell.
paul@server:/$ podman run -it --rm=true debian:11.2
root@9116b919a484:/# cat /etc/os-release 
PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
NAME="Debian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
root@9116b919a484:/# exit
exit
pgrozav@cvst-pgrozav:/$ 

```
