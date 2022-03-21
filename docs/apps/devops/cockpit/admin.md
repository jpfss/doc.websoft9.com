---
sidebar_position: 2
slug: /cockpit/admin
tags:
  - 故障
  - 路径
---


# 维护参考

## Cockpit 子目录配置域名

Cockpit 配置域名有一定的特殊之处（[方案来源](https://caddy.community/t/example-cockpit/8283)）：

1. 先修改/etc/cockpit/cockpit.conf

```
[WebService]
Origins = https://example.com wss://example.com
ProtocolHeader = X-Forwarded-Proto
UrlRoot=/cockpit
```

2. 然后配置 proxy （以 Caddy 为例）
```
example.com {
    reverse_proxy /cockpit/* localhost:9090 {
        transport http {
            tls_insecure_skip_verify
        }
    }
}
```

## 故障处理{#troubleshooting}
## 常见问题{#faq}
## 参数{#parameter}