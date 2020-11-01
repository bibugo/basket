---
layout: post
title:  "Wireguard Warning conf is world accessible"
date:   2020-11-01 14:29:00 +0800
---

```bash
wg-quick up wg0
```

If a message like the following is shown in this step:

```
Warning: `/etc/wireguard/wg0.conf' is world accessible
```

This means that the configuration file permissions are too broad - and they shouldn’t, as there’s a private key in there. This can be fixed with

```bash
chmod 600 /etc/wireguard/wg0.conf
```
