---
layout: post
title:  "Openwrt network interface alias and iproute2"
date:   2020-10-22 10:50:00 +0800
---

1. ip address alias

`/etc/config/network`

```
config interface 'lan2'
	option proto 'static'
	option ifname '@lan'
	option netmask '255.255.255.0'
	option ipaddr '192.168.5.253'
```

2. set ip address route rule for second ethernet interface

```bash
echo "1	lans" >> /etc/iproute2/rt_tables
```

`/etc/config/network`

```
config interface 'lans'
	option proto 'static'
	option ifname 'eth1'
	option netmask '255.255.255.0'
	option ipaddr '192.168.8.211'

config rule
	option src '192.168.8.0/24'
	option lookup '1'

config route
	option interface 'lans'
	option target '0.0.0.0/0'
	option gateway '192.168.8.1'
	option table '1'
```
