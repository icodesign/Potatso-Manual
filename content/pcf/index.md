+++
title = "PCF (Potatso Config File)"
date = "2017-03-22T23:20:50+08:00"

+++

Potatso 支持使用文本来定义用户配置、代理、规则集，以便于归档或分享配置，简称为 PCF（Potatso Configuration File)。

PCF 以 [TOML](https://github.com/toml-lang/toml) 为基础，语法简洁，自解释。

## 示例

以下是一个 PCF 示例：

```toml
# 头部描述信息（可选）
# 在 PCL 的头部一般添加上本文件和作者的描述信息，在 Potatso App 中暂时没有实际作用。
name = "Potatso Sample Configuration"
author = "Potatso"
email = "potatso.com@gmail.com"
website = "http://manual.potatso.com"
description = "The sample PCF. This demonstrates the basic grammar of defining a PCL."

# 用户配置（Profile）
[PROFILE.sample]
name = "Sample Profile"
dns = [
  "8.8.8.8",
  "223.5.5.5"
]
default = "PROXY"
proxy = "hk"
rulesets = [
    "direct",
    "google"
]

# 代理（Proxy）
[PROXY.hk]
type = "SHADOWSOCKS"
host = "ss.example.com"
port = 8388
encryption = "aes-256-cfb"
password = "DO NOT EXPOSE IT TO OTHERS"
remark = "HK Proxy"

[PROXY.us_ssr]
type = "SHADOWSOCKSR"
host = "ssr.example.com"
port = 8388
encryption = "aes-256-cfb"
password = "DO NOT EXPOSE IT TO OTHERS"
protocol = "auth_aes128_md5"
protocolParam = "64#123:user"
obfs = "tls1.2_ticket_auth"
obfsParam = "cloudflare.com"
remark = "HK Proxy"

[PROXY.jp]
type = "SOCKS5"
host = "socks5.example.com"
port = 8888
user = "testuser"
password = "DO NOT EXPOSE IT TO OTHERS"
remark = "HK Proxy"

# 规则集（Ruleset）
[RULESET.direct]
name = "Direct"
rules = [
    "DOMAIN-SUFFIX, cn, DIRECT",
    "GEOIP, cn, DIRECT"
]

[RULESET.google]
name = "Google Rules"
rules = [
    "DOMAIN-MATCH, google, PROXY",
    "DOMAIN-MATCH, gstatic, PROXY"
]
```

## 头部描述
在 PCF 头部，你可以添加必要的说明信息，包括但不仅限于以下字段：

字段 | 描述 | 值类型
---- | ---- | -----
name | PCF 名称 | 字符串
author | 作者名称 | 字符串
email | 作者 Email | 字符串
website | 作者网站地址 | 字符串
description | 描述 | 字符串


> 头部描述是可选的，但我建议公开的 PCF 填写以上信息，便于传播宣传以及用户反馈。

## 区块声明
Potatso 通过区块声明分隔用户配置、代理、规则集。其语法为：

```
[SECTION.id]
```

`SECTION` 表示区块名称，要求大写，目前支持的区块为 用户配置 PROFILE, 代理PROXY, 规则集 RULESET

`id` 是区块在当前 PCF 内的唯一标识，用于跨区块引用。

## 用户配置
Potatso 在运行时依赖一个用户配置，它包含通用配置、代理、规则集。
 
 字段 | 描述 | 值类型
---- | ---- | -----
name |配置名称 | 字符串
dns | 自定义 DNS 列表 | 字符串列表
default | 默认的代理 id，如无默认直连可设置为 direct 或不添加该字段 | 字符串
rulesets | 自定义规则集 id 列表 | 字符串列表

## 代理

目前 PCF 支持两种方式定义代理：

### 1. URL
不同代理对应的 URL 请参照

 字段 | 描述 | 值类型
---- | ---- | -----
url | 代理的 URL 表示 | 字符串

### 2. 多字段表示

 字段 | 描述 | 值类型
---- | ---- | -----
host | 服务器地址 | 字符串
port | 服务器端口 | 整数
user | 用户名（可选） | 字符串
password | 密码 | 字符串
encryption | 加密方法（可选，适用于 Shadowsocks 类代理） | 字符串

{{< warning title="注意安全" >}}
由于代理信息（尤其是服务器地址和密码）必须明文表示，如无必要，请不要公开分享。
{{< /warning >}}

## 规则集
 字段 | 描述 | 值类型
---- | ---- | -----
name | 规则集名称 | 字符串
rules | 规则列表（其中每一项为字符串表示的规则） | 字符串列表

## 注释
以 # 开头的行表示注释，用于提供必要的说明。

------------
## 从其它配置转换

### 1. Surge 规则转成 PCF

### 2. PAC 转成 PCF
