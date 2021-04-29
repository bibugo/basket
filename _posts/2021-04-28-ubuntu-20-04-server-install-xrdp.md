---
layout: post
title:  "Ubuntu 20.04 server install xrdp"
date:   2021-04-28 14:01:00 +0800
---

```bash
sudo apt update
sudo apt install xubuntu-desktop
sudo apt install xrdp
sudo adduser xrdp ssl-cert
echo "startxfce4" > ~/.xsession
chmod +x ~/.xsession
sudo systemctl restart xrdp
sudo apt purge light-locker  #when select gdm3 but not lightdm, remove it to prevent error
```


# /etc/netplan/00-installer-config.yaml
```
network:
  ethernets:
    ens160:
      dhcp4: true
  version: 2
  renderer: NetworkManager
```
  
# /etc/polkit-1/localauthority/50-local.d/45-allow-colord.pkla
```
[Allow Colord all Users]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profile
ResultAny=no
ResultInactive=no
ResultActive=yes
```


