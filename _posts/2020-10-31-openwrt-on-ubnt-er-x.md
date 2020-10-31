---
layout: post
title:  "OpenWrt on ubnt er-x"
date:   2020-10-31 22:15:00 +0800
---

# TFTP Recovery

REF: [EdgeRouter - TFTP Recovery](https://help.ui.com/hc/en-us/articles/360019289113-EdgeRouter-TFTP-Recovery)

1) Download firmware

[ER-X-SFP / ER-X / EP-R6 (e50)](https://dl.ui.com/firmwares/edgemax/v2.0.x/ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed)

2) Enter recovery mode

-  (power unplug)
- connect pc to port `eth0`
- set pc ip address 192.168.1.10/24
- push and hold reset button, plugging power cable
- port led light up sequence from eth0 to eth4
- continue hold reset and wait about 30 seconds until all port led light up
- release button, and wait
- TFTP recovery mode active: `eth0,eth2,eth4` and `eth1,eth3` continuously turn on and off

3) Upload firmware

- on macos

```bash
tftp
tftp> connect 192.168.1.20
tftp> binary
tftp> put ER-e50.recovery.v2.0.6.5208541.190708.0508.16de5fdde.img.signed
```

- when uploading, port led light up form eth0 to eth4
- wait for the TFTP recovery process to completed
- system will automatically reboot
- access via 192.168.1.1 ip address on the eth0 port
- default username/password: ubnt

# Installation OpenWrt

REF1: [Ubiquiti EdgeRouter X (ER-X)](https://openwrt.org/toh/ubiquiti/ubiquiti_edgerouter_x_er-x_ka)
REF2: [(Chinese)](https://lisongmin.github.io/openwrt-erx/)

1) download firmware

`image does not support the device` when use new version in REF1, download this old version:

[lede-ramips-mt7621-ubnt-erx-initramfs-factory.tar](https://www.freifunk-winterberg.net/wp-content/uploads/2017/07/lede-ramips-mt7621-ubnt-erx-initramfs-factory.tar) ( [Mirror](https://www.geefire.eu.org/static/lede-ramips-mt7621-ubnt-erx-initramfs-factory.tar) )

2) upload to er-x

```bash
scp lede-ramips-mt7621-ubnt-erx-initramfs-factory.tar ubnt@192.168.1.1:/tmp/
```

3) ssh in er-x and add image

```bash
ssh ubnt@192.168.1.1
cd /tmp
add system image lede-ramips-mt7621-ubnt-erx-initramfs-factory.tar
show system image
reboot
```
ignore warning `too many arguments`

4) download openwrt sysupgrade file

from REF1, or this [link](http://downloads.openwrt.org/releases/19.07.4/targets/ramips/mt7621/openwrt-19.07.4-ramips-mt7621-ubnt-erx-squashfs-sysupgrade.bin)

5) upload to exr-x

connect pc to `eth1`

```bash
scp openwrt-19.07.4-ramips-mt7621-ubnt-erx-squashfs-sysupgrade.bin root@192.168.1.1:/tmp/
```

6) upgrade

```bash
ssh root@192.168.1.1
cd /tmp
sysupgrade openwrt-19.07.4-ramips-mt7621-ubnt-erx-squashfs-sysupgrade.bin
```

---

on openwrt, `Network` - `Switch`, port map as:
```
LAN1 - LAN2 - LAN3 - LAN4    -    WAN
eht1 - eth2 - eth3 - eth4    -    eth0
                (right port) - (left port)
```
