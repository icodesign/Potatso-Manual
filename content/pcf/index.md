+++
title = "PCF (Potatso Config File)"
date = "2017-03-22T23:20:50+08:00"

+++

## Hello 

Still in progress...

## Sample

```toml
[PROFILE.default]
name = "Sample Profile1"
dns = [
  "8.8.8.8",
  "223.5.5.5"
]
defaultRoute = "PROXY"
defaultProxy = "ssr"
rulesets = [
	"direct",
    "gfw"
]

[PROFILE.direct]
name = "Sample Profile2"
defaultRoute = "DIRECT"
defaultProxy = "ss"

[PROXY.ss]
type = "shadowsocks"
host = "ss.example.com"
port = 8388
password = "DO NOT EXPOSE IT TO OTHERS"
encryption = "aes-256-cfb"
remark = "Test SS"

[PROXY.ssr]
type = "shadowsocksR"
host = "ssr.example.com"
port = 8388
password = "DO NOT EXPOSE IT TO OTHERS"
encryption = "aes-256-cfb"
obfs = "http_simple"
obfsParam = "google.com"
protocol = "auth_aes128_sha1"
protocolParam = "baidu.com"
remark = "Test SSR"

[RULESET.direct]
name = "Test Direct"
rules = [
    "GEOIP, CN, DIRECT",
    "DOMAIN-SUFFIX, CN, DIRECT"
]

[RULESET.gfw]
name = "Test GFW"
rules = [
    "DOMAIN, google, Proxy",
    "DOMAIN-SUFFIX, instagram.com, PROXY",
    "IP-CIDR, 192.0.0.0/8, REJECT",
    "DOMAIN-MATCH, facebook, proxy"
]
```