---
layout: post
title:  "ESXI 7 modify host domain and regenerate certificate"
date:   2020-11-05 15:31:00 +0800
---

# modify host name and domain name

`Networking` - `TCP/IP stacks` - `Default TCP/IP stack` - `(pencil icon)`

modify "Host name" / "Domain name" / "search domains"

OR use command line: `esxcli system hostname`

# generate certificate

ssh root@your.esxi.host.ip

```bash
cd /etc/vmware/ssl

mv rui.crt orig.rui.crt   //backup
mv rui.key orig.rui.key

/sbin/generate-certificates

services.sh restart   //restart service
```

`/sbin/generate-certificates` set the validity days 4200, it's cause chrome error `NET::ERR_CERT_VALIDITY_TOO_LONG`
```bash
cp /sbin/generate-certificates generate-certificates   // file is writing protected
vi generate-certificates   // change `4200` to `365` (3 positions)
./generate-certificates
```

#trust ca
`cat /etc/vmware/ssl/castore.pem`
