---
layout: post
title:  "ros routeros nat return traffic"
date:   2020-10-13 12:05:00 +0800
---

```
/ip cloud
set ddns-enabled=yes
# when active, record the `DNS Name`, ex: `snsnsn.sn.mynetname.net`

/ip firewall address-list
add address=`snsnsn.sn.mynetname.net` list=`wan-ip`

add action=dst-nat chain=dstnat dst-address-list=`wan-ip` dst-port=50000 \
    protocol=tcp to-addresses=192.168.0.10 to-ports=443

```
