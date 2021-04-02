---
layout: post
title:  "VSCode remote container build with no-cache"
date:   2021-04-02 20:19:00 +0800
---

```bash
#in host(macos) terminal
cd .devcontainer
docker build --no-cache .          # build the image without using the local cached layers
docker build --no-cache --pull .   # also pull new base image (FROM)
# after build the image, reopen remote container in the vscode
```
