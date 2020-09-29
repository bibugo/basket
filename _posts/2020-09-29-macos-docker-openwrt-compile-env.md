---
layout: post
title:  "macOS docker openwrt compile env"
date:   2020-09-29 13:30:00 +0800
---

```bash
export UID=$(id -u)
export GID=$(id -g)
docker run -itd --name ubuntu -p 1022:22 -v ~/ubuntu:/home/$USER/macos --user $UID:$GID --workdir="/home/$USER" ubuntu:18.04

docker exec -it -u 0 -e USER=$USER -e UID=$UID -e GID=$GID ubuntu bash
```

```bash
# run in container
useradd -m $USER --uid=$UID --gid=$GID
id $USER
cp /etc/skel/.bash_logout .
cp /etc/skel/.bashrc .
cp /etc/skel/.profile .
chown -R $UID:$GID .
passwd $USER
apt-get update
apt-get install sudo
usermod -aG sudo $USER
^D
```

```bash
docker exec -it ubuntu bash
```

```bash
# run in container
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync
^D
```

```bash
docker stop ubuntu
docker ps -a
docker commit ubuntu ubuntu:openwrt
docker container rm ubuntu
docker run -itd --name ubuntu -p 1022:22 -v ~/ubuntu:/home/$USER/macos --user $UID:$GID --workdir="/home/$USER" ubuntu:openwrt
docker exec -it ubuntu bash
```
