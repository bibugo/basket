---
layout: post
title:  "Configure sudo without password using key authentication"
date:   2020-10-16 11:07:00 +0800
---

login as `root`

edit sudoers file:

```bash
chmod +w /etc/sudoers
vi /etc/sudoers
chmod -w /etc/sudoers
```

change the following line:

```
# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) ALL
```

to

```
# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) NOPASSWD:ALL
```
