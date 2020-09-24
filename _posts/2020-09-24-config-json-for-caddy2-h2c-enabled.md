---
layout: post
title:  "Config.json for caddy2 h2c enabled as trojan remote"
date:   2020-09-24 12:15:00 +0800
---

```json
{
    "admin": {
        "disabled": true
    },
    "logging": {
        "logs": {
            "default": {
                "exclude": [
                    "http.log.access.log0"
                ]
            },
            "log0": {
                "writer": {
                    "filename": "/var/log/caddy/access.log",
                    "output": "file"
                },
                "encoder": {
                    "field": "common_log",
                    "format": "single_field"
                },
                "include": [
                    "http.log.access.log0"
                ]
            }
        }
    },
    "apps": {
        "http": {
            "servers": {
                "srv0": {
                    "listen": [
                        "127.0.0.1:8080"
                    ],
                    "routes": [
                        {
                            "handle": [
                                {
                                    "handler": "vars",
                                    "root": "/var/www/html"
                                }
                            ]
                        },
                        {
                            "match": [
                                {
                                    "header": {
                                        "Connection": [
                                            "*Upgrade*"
                                        ],
                                        "Upgrade": [
                                            "websocket"
                                        ]
                                    },
                                    "path": [
                                        "/ws"
                                    ]
                                }
                            ],
                            "handle": [
                                {
                                    "handler": "subroute",
                                    "routes": [
                                        {
                                            "handle": [
                                                {
                                                    "handler": "rewrite",
                                                    "strip_path_prefix": "/ws"
                                                },
                                                {
                                                    "handler": "reverse_proxy",
                                                    "headers": {
                                                        "request": {
                                                            "delete": [
                                                                "Origin"
                                                            ]
                                                        }
                                                    },
                                                    "upstreams": [
                                                        {
                                                            "dial": "127.0.0.1:8001"
                                                        }
                                                    ]
                                                }
                                            ]
                                        }
                                    ]
                                }
                            ]
                        },
                        {
                            "handle": [
                                {
                                    "handler": "file_server",
                                    "hide": [
                                        "Caddyfile"
                                    ]
                                }
                            ]
                        }
                    ],
                    "automatic_https": {
                        "disable_redirects": true
                    },
                    "logs": {
                        "default_logger_name": "log0"
                    },
                    "allow_h2c": true,
                    "experimental_http3": true
                },
                "srv1": {
                    "listen": [
                        "127.0.0.1:8443"
                    ],
                    "routes": [
                        {
                            "match": [
                                {
                                    "host": [
                                        "YOUR.DOMAIN.COM"
                                    ]
                                }
                            ],
                            "handle": [
                                {
                                    "handler": "subroute",
                                    "routes": [
                                        {
                                            "handle": [
                                                {
                                                    "body": "Not Found",
                                                    "handler": "static_response",
                                                    "status_code": 404
                                                }
                                            ]
                                        }
                                    ]
                                }
                            ],
                            "terminal": true
                        }
                    ],
                    "automatic_https": {
                        "disable_redirects": true
                    },
                    "experimental_http3": true
                }
            }
        },
        "tls": {
            "automation": {
                "policies": [
                    {
                        "subjects": [
                            "YOUR.DOMAIN.COM"
                        ],
                        "issuer": {
                            "challenges": {
                                "dns": {
                                    "provider": {
                                        "api_token": "TOKENTOKENTOKENTOKENTOKENTOKENTOKEN",
                                        "name": "cloudflare"
                                    }
                                }
                            },
                            "module": "acme"
                        }
                    },
                    {
                        "issuer": {
                            "challenges": {
                                "bind_host": "127.0.0.1"
                            },
                            "module": "acme"
                        }
                    }
                ]
            }
        }
    }
<<<<<<< HEAD
}
```
  
  
# this file is generate from Caddyfile
not use Caddyfile, because Caddyfile didn't support `allow_h2c` yet.
```
{
=======
  }
  ```
  
  
  this file is generate from Caddyfile
  ```json
  {
>>>>>>> parent of 009fe5a... Update 2020-09-24-config-json-for-caddy2-h2c-enabled.md
  admin off
  auto_https disable_redirects
  experimental_http3
}

:8080 {
  bind 127.0.0.1

  log {
    output file /var/log/caddy/access.log
    format single_field common_log
  }

  @websockets {
    header Connection *Upgrade*
    header Upgrade websocket
    path /ws
  }

  handle @websockets {
    uri strip_prefix /ws
    reverse_proxy 127.0.0.1:8001 {
      header_up -Origin
    }
  }

  root * /var/www/html
  file_server
}

https://YOUR.DOMAIN.COM:8443 {
  bind 127.0.0.1
  tls {
    dns cloudflare TOKENTOKENTOKENTOKENTOKENTOKENTOKEN
  }
  respond "Not Found" 404
}
  ```
