---
layout: post
title:  "Openwrt ssr plus cannot update data with wget"
date:   2020-12-30 14:17:00 +0800
---

when compile **ssr plus** with openwrt snapshot version,

if openwrt package **wget** in feed 

**src-git packages https://git.openwrt.org/feed/packages.git** in __feeds.conf.default__
(local in __feeds/packages/net/wget__)

is **newer**, ssr plus update data not work!

# just replace **wget** package with the same package in lean source!
