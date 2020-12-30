---
layout: post
title:  "Openwrt snapshot compile enable i2c for nanopi neo2"
date:   2020-12-30 13:35:15 +0800
---

openwrt snapshot with new kernel has removed **kmod-i2c-gpio-custom** module, it's not needed, but **i2c** is disabled default

# modify dts to enable i2c0

```bash
vi ./build_dir/target-aarch64_cortex-a53_musl/linux-sunxi_cortexa53/linux-5.4.85/arch/arm/boot/dts/sunxi-h3-h5.dtsi
```

set i2c0 status to **"okay"**


# OR, 

```bash
vi ./build_dir/target-aarch64_cortex-a53_musl/linux-sunxi_cortexa53/linux-5.4.85/arch/arm64/boot/dts/allwinner/sun50i-h5-nanopi-neo2.dts
```
and add follow lines
```
&i2c0 {
        status = "okay";
};
```

and than, **RE** compile!

***

# openwrt compile for nanopi neo2 with NanoHatOLED

add ***src-git NanoHatOLED https://github.com/vinewx/NanoHatOLED.git*** to ***feeds.conf.default***

```
./scripts/feeds update NanoHatOLED && ./scripts/feeds install nanohatoled
```

```
make menuconfig
```

select package ___Extra packages -> nanohatoled___

be sure depends are selected:
```
i2c-tools
kmod-i2c-core
kmod-i2c-gpio
kmod-i2c-smbus (not sure if needed)
python3-pillow
python3-smbus
```

# AND be careful, temperature display maybe not work when press K2

if not work K2, disable temperature display in python file

***nanohatoled/files/NanoHatOLED/bakebit_nanohat_oled.py*** (installed @ /usr/share/NanoHatOLED/bakebit_nanohat_oled.py)

comment and set **tempStr** to empty
```
#        tempI = int(open('/sys/class/thermal/thermal_zone0/temp').read());
#        if tempI>1000:
#            tempI = tempI/1000
#        tempStr = u"CPU TEMP: %s\u00b0C" % int(tempI)
        tempStr = ""
```
