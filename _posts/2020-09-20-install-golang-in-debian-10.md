---
layout: post
title:  "Install Golang in Debian 10"
date:   2020-09-20 09:58:15 +0800
---

```bash
# install GO
wget https://golang.org/dl/go1.15.2.linux-amd64.tar.gz
tar  -zxvf  go1.15.2.linux-amd64.tar.gz  -C  /usr/local/
vi /etc/profile
···
export GOROOT=/usr/local/go
export PATH=$GOROOT/bin:$PATH
···
source /etc/profile
go version
go env
```
