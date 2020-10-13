---
layout: post
title:  "dsm6.x add 2nd ip address with vlanid"
date:   2020-10-13 09:09:00 +0800
---

`/usr/local/etc/rc.d/start_interface_vlan30.sh`

```bash
#!/bin/sh

ovs-vsctl add-br ovs2_bond0 ovs_bond0 30
# ovs_bond0 main interface(bond with open vswitch)
# ovs2_bond0 interface name to add
# 30 vlan id
ip addr add 11.30.0.5/24 brd 11.30.0.255 dev ovs2_bond0
# set ip address
ip link set ovs2_bond0 up
# link up
```

# `PS.`

add docker macvlan network for interfaces

```bash
docker network create -d macvlan --subnet=11.0.0.0/24 --gateway=11.0.0.1 -o parent=ovs_bond0 macvlan1
docker network create -d macvlan --subnet=11.30.0.0/24 --gateway=11.30.0.1 -o parent=ovs2_bond0 macvlan30
# docker run -d --net=macvlan30 --ip=11.30.0.80 ...
```
