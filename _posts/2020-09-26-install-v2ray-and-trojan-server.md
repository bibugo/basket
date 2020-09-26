---
layout: post
title:  "Install v2ray and trojan server"
date:   2020-09-26 21:40:15 +0800
---

1. install v2ray server
[official repo](https://github.com/v2fly/v2ray-core)
[install script](https://github.com/v2fly/fhs-install-v2ray)

```bash
curl -LROJ https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
bash install-release.sh
```

2. install trojan server
[official repo](https://github.com/trojan-gfw/trojan)

```bash
sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"
```
