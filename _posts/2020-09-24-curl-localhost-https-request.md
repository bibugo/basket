---
layout: post
title:  "Curl specify hostname for localhost https request"
date:   2020-09-24 11:35:15 +0800
---

# Curl specify hostname for localhost https request

```bash
curl --verbose --resolve your.domain.com:8443:127.0.0.1 https://your.domain.com:8443
```
