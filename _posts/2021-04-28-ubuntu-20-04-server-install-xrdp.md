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
sudo apt purge light-locker
```
