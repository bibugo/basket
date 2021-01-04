---
layout: post
title:  "Resolve parallels 16 big sur network initialization failed"
date:   2021-01-04 10:47:00 +0800
---

# Network Fix

```bash
sudo vi /Library/Preferences/Parallels/network.desktop.xml
```

change **<UseKextless>X</UseKextless>** to
```
<UseKextless>0</UseKextless>
```
if not exist, add to `ParallelsNetworkConfig` section

# USB Fix

```bash
sudo vi /Library/Preferences/Parallels/dispatcher.desktop.xml
```

change **<Usb>0</Usb>** to
```
<Usb>1</Usb>
```

# upgrade is ok
