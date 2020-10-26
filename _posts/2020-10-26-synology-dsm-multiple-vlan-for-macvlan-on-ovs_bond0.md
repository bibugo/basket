---
layout: post
title:  "Synology DSM multiple vlan for macvlan on ovs_bond0"
date:   2020-10-26 10:26:00 +0800
---

# VLAN 20

`/etc/sysconfig/network-scripts/ifcfg-ovs_eth19` (eth19 will show with name "LAN 20" in `Control Panel`)

```
BOOTPROTO=static
DEVICE=ovs_eth19
TYPE=OVS
OVS_VLAN_ID=20
IPADDR=192.168.20.5
NETMASK=255.255.255.0
IPV6INIT=off
IPV6_ACCEPT_RA=0
```

# VLAN 30

`/etc/sysconfig/network-scripts/ifcfg-ovs_eth29`

```
BOOTPROTO=static
DEVICE=ovs_eth29
TYPE=OVS
OVS_VLAN_ID=30
IPADDR=10.30.0.5
NETMASK=255.255.255.0
IPV6INIT=off
IPV6_ACCEPT_RA=0
```

# add startup script

`Control Panel > Task Scheduler`

```
ovs-vsctl add-br ovs_eth19 ovs_bond0 20
ip addr add 192.168.20.5/24 brd 192.168.20.255 dev ovs_eth19
ip link set ovs_eth19 up
ovs-vsctl add-br ovs_eth29 ovs_bond0 30
ip addr add 10.30.0.5/24 brd 11.30.0.255 dev ovs_eth29
ip link set ovs_eth29 up
```

# create macvlan network

```bash
docker network create -d macvlan --subnet=10.0.0.0/24 --gateway=10.0.0.1 -o parent=ovs_bond0 macvlan1
docker network create -d macvlan --subnet=192.168.20.0/24 --gateway=192.168.20.1 -o parent=ovs_eth19 macvlan20
docker network create -d macvlan --subnet=10.30.0.0/24 --gateway=10.30.0.1 -o parent=ovs_eth29 macvlan30
```

# add route host to docker container

`Control Panel > Task Scheduler`

```
ip monitor link | grep -q 'ovs_bond0:.*UP,LOWER_UP'
ip link add mac0 link ovs_bond0 type macvlan mode bridge
ip addr add 10.0.0.6/24 dev mac0
ip link set mac0 up
ip route add 10.0.0.10/32 dev mac0
ip route add 10.0.0.52/32 dev mac0
ip route add 10.0.0.53/32 dev mac0
```

# that's all
