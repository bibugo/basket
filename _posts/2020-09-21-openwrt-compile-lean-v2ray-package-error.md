---
layout: post
title:  "Openwrt Compile Lean's V2ray Package Error"
date:   2020-09-21 14:26:00 +0800
---

# copy tools/upx and tools/ucl from lean's lede repository to tools directory
# add the following 2 lines to tools/Makefile
```
tools-y += ucl upx
$(curdir)/upx/compile := $(curdir)/ucl/compile
```
