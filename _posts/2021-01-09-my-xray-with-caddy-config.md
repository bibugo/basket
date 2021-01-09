---
layout: post
title:  "My xray with caddy config"
date:   2021-01-09 11:43:00 +0800
---

# `/usr/local/etc/xray/config.json`

```
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "uuid",
            "flow": "xtls-rprx-direct",
            "level": 0,
            "email": "your@domain"
          }
        ],
        "decryption": "none",
        "fallbacks": [
          {
            "dest": 2443,
            "xver": 1
          },
          {
            "path": "/ws",
            "dest": 2080,
            "xver": 1
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "xtls",
        "xtlsSettings": {
          "alpn": ["h2", "http/1.1"],
          "certificates": [
            {
              "certificateFile": "/usr/local/etc/xray/fullchain.crt",
              "keyFile": "/usr/local/etc/xray/private.key"
            }
          ]
        }
      }
    },
    {
      "port": 2443,
      "listen": "127.0.0.1",
      "protocol": "trojan",
      "settings": {
        "clients": [
          {
            "password": "password",
            "level": 0,
            "email": "your@domain"
          }
        ],
        "fallbacks": [
          {
            "dest": 8080
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "none",
        "tcpSettings": {
          "acceptProxyProtocol": true
        }
      }
    },
    {
      "port": 2080,
      "listen": "127.0.0.1",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "uuid",
            "level": 0,
            "email": "your@domain"
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "none",
        "wsSettings": {
          "acceptProxyProtocol": true,
          "path": "/ws"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom"
    }
  ]
}
```

# `/etc/caddy/Caddyfile`

```
{
  admin off
  auto_https disable_redirects
  servers 127.0.0.1:8080 {
    protocol {
      allow_h2c
    }
  }
}

:80 {
  redir https://domain.name permanent
}

:8080 {
  bind 127.0.0.1
  log {
    output file /var/log/caddy/access.log
    format single_field common_log
  }
  route {
    forward_proxy {
      basic_auth username password
      hide_ip
      hide_via
      probe_resistance
    }
    file_server {
      root /var/www/html
    }
  }
}

https://domain.name:8443 {
  bind 127.0.0.1
  tls {
    dns cloudflare cloudflare_api_token
  }
  respond 204
}
```
