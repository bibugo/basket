---
layout: post
title:  "Openwrt compile open-vm-tools error"
date:   2020-09-28 12:37:00 +0800
---

Openwrt compile with package open-vm-tools throw two error:
```
ld: cannot find -liconv
ld: cannot find -lintl
```
---

* make menuconfig
* check `Global build settings` -> `Compile with full language support` to compile libiconv and libintl
and the errors throw,

but libiconv and libintl had been compiled into `./staging_dir/target-x86_64_musl/usr/lib/libiconv-full` and `libintl-full`.

---

since the lib file .so located in subfolder, it must be linked to `./staging_dir/target-x86_64_musl/usr/lib` manually.


```bash
cd /home/username/openwrt/

find . -name "libiconv.so"
find . -name "libintl.so"

ln -s /home/username/openwrt/staging_dir/target-x86_64_musl/usr/lib/libiconv-full/lib/libiconv.so ./staging_dir/target-x86_64_musl/usr/lib/libiconv.so
ln -s /home/username/openwrt/staging_dir/target-x86_64_musl/usr/lib/libintl-full/lib/libintl.so ./staging_dir/target-x86_64_musl/usr/lib/libintl.so

ls ./staging_dir/target-x86_64_musl/usr/lib/
```

---

ok, make again, the errors gone.

---

`another error`

> Package wget is missing dependencies for the following libraries:

add to `./feeds/packages/net/wget/Makefile`
> DEPENDS:=+libpcre +zlib `+libintl`

and run

```bash
cp ./staging_dir/target-x86_64_musl/pkginfo/libintl-full.provides ./staging_dir/target-x86_64_musl/pkginfo/libintl.provides
```
