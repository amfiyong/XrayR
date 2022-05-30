# XrayR
[![](https://img.shields.io/badge/TgChat-@XrayR讨论-blue.svg)](https://t.me/XrayR_project)
[![](https://img.shields.io/badge/Channel-@XrayR通知-blue.svg)](https://t.me/XrayR_channel)

A Xray backend framework that can easily support many panels.

一个基于Xray的后端框架，支持V2ay,Trojan,Shadowsocks协议，极易扩展，支持多面板对接。

如果您喜欢本项目，可以右上角点个star+watch，持续关注本项目的进展。

## 免责声明

本项目只是本人个人学习开发并维护，本人不保证任何可用性，也不对使用本软件造成的任何后果负责。

## 特点
* 永久开源且免费。
* 支持V2ray，Trojan， Shadowsocks多种协议。
* 支持Vless和XTLS等新特性。
* 支持单实例对接多面板、多节点，无需重复启动。
* 支持限制在线IP
* 支持节点端口级别、用户级别限速。
* 配置简单明了。
* 修改配置自动重启实例。
* 方便编译和升级，可以快速更新核心版本， 支持Xray-core新特性。



## TODO
- [x] 在线用户统计和限制
- [x] 限速实现
- [x] 审计规则
- [ ] 对接ProxyPanel
- [x] 对接v2board

## 功能介绍

| 功能            | v2ray | trojan | shadowsocks |
| --------------- | ----- | ------ | ----------- |
| 获取节点信息    | √     | √      | √           |
| 获取用户信息    | √     | √      | √           |
| 用户流量统计    | √     | √      | √           |
| 服务器信息上报  | √     | √      | √           |
| 自动申请tls证书 | √     | √      | √           |
| 自动续签tls证书 | √     | √      | √           |
| 在线人数统计    | √     | √      | √           |
| 在线用户限制    | √     | √      | √           |
| 审计规则        | √     | √      | √           |
| 节点端口限速    | √     | √      | √           |
| 按照用户限速    | √     | √      | √           |
| 自定义DNS       | √     | √      | √           |
## 支持前端

| 前端        | v2ray | trojan | shadowsocks                    |
| ----------- | ----- | ------ | ------------------------------ |
| sspanel-uim | √     | √      | √ (Shadowsocks - 单端口多用户) |
| ProxyPanel  | TODO  | TODO   | TODO                           |
| v2board     | √     | √      | √                              |

## Thanks

* [Project X](https://github.com/XTLS/)
* [V2Fly](https://github.com/v2fly)
* [VNet-V2ray](https://github.com/ProxyPanel/VNet-V2ray)
* [Air-Universe](https://github.com/crossfw/Air-Universe)

## Licence

[Mozilla Public License Version 2.0](https://github.com/amfiyong/XrayR/blob/main/LICENSE)

## Telgram

[XrayR后端讨论](https://t.me/XrayR_project)

[XrayR通知](https://t.me/XrayR_channel)

# 一键安装
```
bash <(curl -Ls https://raw.githubusercontent.com/amfiyong/XrayR-release/master/install.sh)
```
## 使用Docker部署软件
[Docker部署教程](https://github.com/amfiyong/XrayR-release/blob/master/README.md)

## 下载并使用
1. 在此处，根据自身系统选择合适的版本：[Release](https://github.com/amfiyong/XrayR/releases)
2. 解压压缩包，之后运行：`./XrayR -config config.yml`

## 编译并使用
1. go 1.16.0
2. 依次运行
```bash
git clone https://github.com/amfiyong/XrayR
cd XrayR/main
go mod tidy
go build -o XrayR -ldflags "-s -w"
./XrayR -config config.yml
```
## 配置文件
1. `cp config.yml.example config.yml`
2. nano config.yml

配置文件基本格式，Nodes下可以同时添加多个面板，多个节点配置信息，只需添加相同格式的Nodes item即可。
``` yaml
Log:
  Level: debug # Log level: none, error, warning, info, debug 
  AccessPath: # ./access.Log
  ErrorPath: # ./error.log
DnsConfigPath: # ./dns.json  Path to dns config
Nodes:
  -
    PanelType: "SSpanel" # Panel type: SSpanel
    ApiConfig:
      ApiHost: "https://example.com"
      ApiKey: "123"
      NodeID: 41
      NodeType: V2ray # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request, Default is 5 sec
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: ./cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node1.test.com.key
        Provider: cloudflare # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          CF_API_EMAIL: 
          CF_API_KEY: 
  -
    PanelType: "V2board" # Panel type: SSpanel, V2board
    ApiConfig:
      ApiHost: "https://example.com"
      ApiKey: "123"
      NodeID: 42
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node2.test.com" # Domain to cert
        CertFile: ./cert/node2.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node2.test.com.key
        Provider: cloudflare # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          CF_API_EMAIL: 
          CF_API_KEY:
```
## 前端配置
### 限速说明
1. 节点限速：请在SSpanel的节点限速处填写，单位mbps。
2. 用户限速：请在SSpanel的用户设置处填写，单位mbps。
3. 限速值设为0，则为不限速。
### 审计规则说明
请在前端审计规则处填写任意正则表达式，如 `baidu.com`将屏蔽所有baidu的域名。暂不支持bt协议的审计。
### 自定义DNS配置
XrayR支持为不同节点设置不同的DNS策略，具体方法如下：
1. 编写dns.json文件，此配置与Xray DNS配置完全相同，请查看：https://xtls.github.io/config/base/dns/ 获取帮助。
2. 在`config.yml`中配置`DnsConfigPath`为dns.json的路径。
3. 在所需要启用自定义DNS的节点中，将`EnableDNS: true`设为true。如设为false或者不填则是使用本机DNS。
4. 如果要启用geoip相关配置，请确保`geoip.dat`和`geosite.dat`处于和`config.yml`同一目录。

### V2board 支持
1. 在`config.yml`中配置`PanelType: "V2board"`。
2. V2board只有V2ray节点类型支持审计规则。
3. 启用vless和xtls，请在配置文件中手动启动，V2board不支持在线配置。
### V2ray

| 协议      | 支持情况                                             |
| --------- | ---------------------------------------------------- |
| VMess     | tcp, tcp+tls, ws, ws+tls, h2c, h2+tls                |
| VMessAEAD | tcp, tcp+tls, ws, ws+tls, h2c, h2+tls                |
| VLess     | tcp, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls |
#### SSpanel-uim 节点地址格式
```
IP;监听端口;alterId;(tcp或ws);(tls或xtls或不填);path=/xxx|host=xxxx.com|server=xxx.com|outside_port=xxx|enable_vless=(true或false)
```
alterId设为0，则自动启用VMessAEAD。
#### TCP示例
```
ip;12345;2;tcp;;server=域名
```
```
示例：1.3.5.7;12345;2;tcp;;server=hk.domain.com
```
#### tcp + tls 示例
```
ip;12345;2;tcp;tls;server=域名|host=域名
```
```
示例：1.3.5.7;12345;2;tcp;tls;server=hk.domain.com|host=hk.domain.com
```
#### ws示例
```
ip;80;2;ws;;path=/xxx|server=域名|host=CDN域名
```
```
示例：1.3.5.7;80;2;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```
#### ws + tls 示例
```
ip;443;2;tls;ws;path=/xxx|server=域名|host=CDN域名
```
```
示例：1.3.5.7;443;2;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```
#### 中转端口
在任一配置组合后增加`|outside_port=xxx`,此项为用户连接端口。
```
示例：1.3.5.7;80;2;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com|outside_port=12345
```
#### 启用Vless **(此项为实验性功能)**

在任一配置组合后增加`|enable_vless=true`.
```
示例：1.3.5.7;12345;2;tcp;tls;server=hk.domain.com|host=hk.domain.com|enable_vless=true
```
请开启vless同时务必使用tls或者xtls。
#### 启用xtls **(此项为实验性功能)**
替换tls为xtls
```
示例：1.3.5.7;12345;2;tcp;xtls;server=hk.domain.com|host=hk.domain.com
```
### Trojan
| 协议   | 支持情况 |
| ------ | -------- |
| Trojan | √        |
#### SSpanel-uim 节点地址格式
```
域名或IP;port=监听端口#用户连接端口|host=xx
```
#### 直连示例
```
示例：gz.aaa.com;port=443|host=gz.aaa.com
```
#### 中转示例
```
示例：gz.aaa.com;port=443#12345|host=hk.aaa.com
```
#### 启用xtls **(此项为实验性功能)**
在配置后增加`|enable_xtls=true`.
```
示例：gz.aaa.com;port=443#12345|host=hk.aaa.com|enable_xtls=true
```

### Shadowsocks
| 协议            | 支持情况 | 加密方法                                    |
| --------------- | -------- | ------------------------------------------- |
| ShadowsocksAEAD | √        | aes-128-gcm, aes-256-gcm, chacha20-poly1305 |
#### SSpanel-uim 节点地址格式
* 请注意，节点类型请选择：`Shadowsocks`
* 单端口多用户承载用户加密方式请选择：`aes-128-gcm`, `aes-256-gcm`, `chacha20-ietf-poly1305`三者之一。
* XrayR目前只支持一个单端口多用户承载用户，有多个承载用户时只使用第一个。
```
域名或IP;port=监听端口#连接端口;server=xx
```
#### Shadowsocks 示例
```
示例：gz.aaa.com;port=80#1234;server=gz.aaa.com
```
## Stargazers over time

[![Stargazers over time](https://starchart.cc/amfiyong/XrayR.svg)](https://starchart.cc/amfiyong/XrayR)

