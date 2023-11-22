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

Install docker: <a href="https://docs.docker.com/engine/installation/linux/docker-ce/debian/" target="_blank">https://docs.docker.com/engine/installation/linux/docker-ce/debian/<a/>.

Check this docker registry(hub): <a href="https://hub.docker.com/explore/" target="_blank">https://hub.docker.com/explore/</a>.

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
paul@server:/$
```

#### 2. Running docker in podman
```bash
podman stop -t0 docker_in_podman ;
podman \
  run \
  --name=docker_in_podman \
  -d \
  --rm=true \
  --privileged \
  ` # Expose any ports that you might need - that are then exposed from docker`
  -p 1025:1025 \
  -p 20070:2375 \
  -p 20071:2376 \
  -p 8443:8443 \
  -p 10080:80 \
  -p 10000:10000/udp \
  -e DOCKER_TLS_CERTDIR="" \
  docker.io/docker:20.10.12-dind-alpine3.14 \
  ;
podman logs -f docker_in_podman
```

#### 3. Other podman commands
```bash
# [podman specific] Launches a process in a new user namespace:
podman unshare cat /proc/self/uid_map

# Check if it's a podman pause process:
server:~ $ ps faux | grep ^myuser | grep podman$                                                                                                          
myuser   9144  0.0  0.0  80800   416 ?        S    Nov11   0:00 podman                                                                                                                      
server:~ $ egrep -nirw pause /proc/9144/{status,sched,comm,stat}                                                                             
/proc/9144/status:1:Name:       podman pause                               
/proc/9144/sched:1:podman pause (9144, #threads: 1)          
/proc/9144/comm:1:podman pause                                    
/proc/9144/stat:1:9144 (podman pause) S 1 9143 9143 0 -1 4202496 8770 0 0 0 0 1 0 0 20 0 1 0 118314610 82739200 104 18446744073709551615 94792096985088 94792132131532 140734602984848 140734602980152 139984983891680 0 2147139327 394276871 0 18446744071608946956 0 0 17 5 0 0 0 0 0 94792134232760 94792161715232 94792179630080 140734602987386 140734602987393 140734602987393 140734602989548 0

# ===

# Docker commands (works in a similar way with podman)
# Show available container images
docker images
podman images

# Show layers of given container image
podman image tree docker.io/roundcube/roundcubemail:1.6.3-apache

# Download image
docker pull debian

# Delete image
docker image rm <IMAGE_ID>

# Show containers
docker ps -a

# Create container from image
docker run -it --name="<CONTAINER_NAME>" <IMAGE_ID>
# You can forward ports with -p <HOST_PORT>:<CONTAINER_PORT>
# You can mount folders with -v /host/dir:/container/dest

# Delete container
docker rm <CONTAINER_ID>

# Run image interactively (creates container and removes it on exit)
docker run -it --rm=true <IMAGE_ID>

# Build docker image from Dockerfile, will be accessible by name <IMAGE_NAME>
docker build -t <IMAGE_NAME> .

# Stop container
docker stop <CONTAINER_ID>

# Start existing container

docker start <CONTAINER_ID>

# List volumes
docker volume ls

# Rename container
docker rename <CURRENT_CONTAINER_NAME> <NEW_CONTAINER_NAME>

# Add docker network
docker network create --subnet=172.18.0.0/16 <NETWORK_NAME>

# List docker networks
docker network list

# Delete docker network
docker network remove <NETWORK_ID>

# Run image (by starting new container) with given IP in given network
docker run --net <NETWORK_NAME> --ip 172.18.0.2 -it --rm=true <IMAGE_NAME>

# Run image (by starting new container) container with given host name
docker run -it --rm=true -h <HOST_NAME> <IMAGE_NAME>

# Rename container
docker rename <CONTAINER_ID> <NEW_CONTAINER_NAME>

# Update a running container to auto-restart
docker update --restart=always <CONTAINER_ID>

# Save docker image to file:
docker save -o <PATH_TO_FILE> <IMAGE_NAME>

# Load docker image from file:
docker load -i <PATH_TO_FILE>

# Remove all podman data(images, container, volumes, etc)
podman system reset

# ============================================================================ #
# This seems like a nice UI for docker:
docker run -d -p 10086:10086 -v /var/run/docker.sock:/var/run/docker.sock tobegit3hub/seagull
# Then go to http://127.0.0.1:10086
# Or you can use portainer: https://www.portainer.io/
# ============================================================================ #
# Clean up your package manager when building images, to keep them small !

# To undo apt-get update you can:
rm -Rf /var/lib/apt/lists/*
# or, even better
apt clean

# Run GUI apps in containers
# Version 1(local):
# It seems that doing "xhost +local:paul" and a "xhost -..." after, is not needed !
podman run -it --rm -v /tmp/.X11-unix:/tmp/.X11-unix:rw -e DISPLAY alpine:3.16.2 /bin/sh -c "apk add xeyes && xeyes"

# Version 2(local):
# Local with net=host and without xhost permissions but requires .Xauthority
# (also works over ssh -X with X11 fwd)
docker run -it --rm -v ${HOME}/.Xauthority:/root/.Xauthority -e DISPLAY --net host debian bash -c "apt update && apt install -y x11-apps && xeyes"
```

#### 4. podman compose
```yml
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
# Start with:
# podman-compose up -d
# Check that it worked with:
# podman-compose logs client
# Stop with:
# podman-compose down -t0
# ============================================================================ #
---
version: '3'
services:
  server:
    image: docker.io/nginx:1.25.2-alpine3.18-slim
    restart: unless-stopped
  client:
    image: docker.io/alpine:3.18.3
    restart: unless-stopped
    command: |-
      sh -c "
        wget -O- http://server:80/
        sleep infinity
      "
# ============================================================================ #
```

#### 5. podman play kube
```yml
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
# Pod defined in kubernetes way.
# Start:
# podman play kube play_kube.yml
# Check result:
# podman logs foobar-client
# Stop:
# podman play kube --down play_kube.yml
# ============================================================================ #
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: foo-env-var
data:
  FOO: bar
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: foo-cfg
data:
  foo.cfg: |
    # My configuration file
    hostname: google.com
    listen.port: 8122
    listen.address: 0.0.0.0
---
apiVersion: v1
kind: Pod
metadata:
  name: foobar
spec:
  volumes:
  - name: vol1
    configMap:
      name: foo-cfg
  - name: vol2
    readOnly: true
    hostPath:
      type: Directory
      path: /home
  containers:
  - name: server
    image: docker.io/nginx:1.25.2-alpine3.18-slim
  - name: client
    image: docker.io/alpine:3.18.3
    command:
    - sh
    - -c
    - |-
        echo "Env var from ConfigMap: FOO="${FOO} &&
        ls -la /root/mnt &&
        cat /root/mnt/foo.cfg &&
        ls -la /root/mnt2 &&
        wget -O- http://127.0.0.1:80/ &&
        sleep infinity
    envFrom:
    - configMapRef:
        name: foo-env-var
        optional: false
    volumeMounts:
    - name: vol1
      mountPath: /root/mnt
    - name: vol2
      mountPath: /root/mnt2
# ============================================================================ #
```

#### 6. Podman uses VFS storage driver by default on Debian 12.1
VFS seems to use a lot of disk space:
```bash
paul@alice:~$ du -hcs /home/paul/.local/share/containers/storage/vfs/dir/* | sort -hr
du: cannot access '/home/paul/.local/share/containers/storage/vfs/dir/*': No such file or directory
0       total
paul@alice:~$ podman pull docker.io/gogs/gogs:0.13.0
Trying to pull docker.io/gogs/gogs:0.13.0...
Getting image source signatures
Copying blob 0ebc3ae9b1fd done
Copying blob 0c5ce5d30fd2 done
Copying blob 63b65145d645 done
Copying blob 64ce99365c40 done
Copying blob 8a60cbfeaf08 done
Copying blob 592edf640933 done
Copying blob b2fba8b9ffe9 done
Copying config 4fa5cedc03 done
Writing manifest to image destination
Storing signatures
4fa5cedc038517695cf354561e6eceec91d1803bc8b8d84744c796f2e47993b4
paul@alice:~$ du -hcs /home/paul/.local/share/containers/storage/vfs/dir/* | sort -rh
333M    total
96M     /home/paul/.local/share/containers/storage/vfs/dir/9d975fa58001e89382abbb36d13348b2df9ee681eac1d2da40b66b9f84c83edc
94M     /home/paul/.local/share/containers/storage/vfs/dir/2dad71847a365bab9b32bb1bc0b588fbe9b91873658808e99f2897461ab244f4
34M     /home/paul/.local/share/containers/storage/vfs/dir/cd208f2c4da0a76b6fc593975bc070c80e3b2033a041dfdb626a17e1124d024e
34M     /home/paul/.local/share/containers/storage/vfs/dir/bdc0a44bef096f2e2ba65a3da759a57ce38397b8c09fab104ef51dec0bee0f04
34M     /home/paul/.local/share/containers/storage/vfs/dir/721f55cb9273f4e0f80a19a2842242cf577ce00b4effc9341e00b08048517d13
34M     /home/paul/.local/share/containers/storage/vfs/dir/297c232b1f58420ed825219f2d4bd8871f54334bee29d0e10da234ccd5d6540b
7.4M    /home/paul/.local/share/containers/storage/vfs/dir/7cd52847ad775a5ddc4b58326cf884beee34544296402c6292ed76474c686d39
paul@alice:~$ podman image tree docker.io/gogs/gogs:0.13.0
Image ID: 4fa5cedc0385
Tags:     [docker.io/gogs/gogs:0.13.0]
Size:     98MB
Image Layers
├── ID: 7cd52847ad77 Size: 7.338MB
├── ID: bdc0a44bef09 Size: 25.52MB
├── ID: cd208f2c4da0 Size:  2.56kB
├── ID: 721f55cb9273 Size: 2.048kB
├── ID: 297c232b1f58 Size: 39.94kB
├── ID: 2dad71847a36 Size: 62.79MB
└── ID: 9d975fa58001 Size: 2.307MB Top Layer of: [docker.io/gogs/gogs:0.13.0]

paul@alice:~$ ls -la /home/paul/.local/share/containers/storage/vfs/dir/721f55cb9273f4e0f80a19a2842242cf577ce00b4effc9341e00b08048517d13
total 80
dr-xr-xr-x 20 paul paul 4096 Oct  4 13:42 .
drwx------  9 paul paul 4096 Oct  4 13:42 ..
drwxr-xr-x  3 paul paul 4096 Feb 25  2023 app
drwxr-xr-x  2 paul paul 4096 Feb 25  2023 bin
drwxr-xr-x  2 paul paul 4096 Feb 10  2023 dev
drwxr-xr-x 23 paul paul 4096 Feb 25  2023 etc
drwxr-xr-x  2 paul paul 4096 Feb 10  2023 home
drwxr-xr-x 10 paul paul 4096 Feb 25  2023 lib
drwxr-xr-x  5 paul paul 4096 Feb 10  2023 media
```
You can see that the layers actually add up using more disk space than it
should.

Solution:
```bash
# Configure podman from scratch
podman system reset &&
rm -rf \
  ${HOME}/.local/share/containers \
  ${HOME}/.config/cni \
  rm -rf ${HOME}/.config/containers \
  &&
mkdir -p ${HOME}/.config/containers &&
( cat - <<EOF > ${HOME}/.config/containers/storage.conf
[storage]
driver = "overlay"

[storage.options.overlay]
mount_program = "/usr/bin/fuse-overlayfs"
EOF 
) &&
# After reset configure overlay as the storage driver
podman info --storage-driver=overlay --debug --log-level=debug
podman info | grep graphDriverName
```