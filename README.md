<p align="center"><img src="https://github.com/NetchX/Netch/blob/main/Netch/Resources/Netch.png?raw=true" width="128" /></p>

<div align="center">

# Netch
A simple proxy client — forked from [netchx/netch](https://github.com/netchx/netch)

[![Build](https://github.com/lbbboy/netch/actions/workflows/build-httpupgrade.yml/badge.svg)](https://github.com/lbbboy/netch/actions/workflows/build-httpupgrade.yml)
[![](https://img.shields.io/github/v/release/netchx/netch?style=flat-square&label=upstream)](https://github.com/netchx/netch/releases)

</div>

## 与上游的差异

### 新增传输协议
- **HTTPUpgrade** — 基于 HTTP/1.1 Upgrade 的传输，适合套 CDN
- **xhttp** — 基于 HTTP/1.1 分块传输

### 新增 TLS 安全类型
- **Reality** — 支持 VLESS + Reality，需填写 Public Key、Short ID、Fingerprint

### 内核替换
- 将 `v2ray-sn.exe`（SagerNet/v2ray-core）替换为 `xray.exe`（XTLS/Xray-core 最新版）
- 原因：SagerNet 分支依赖 `sagernet/gvisor`，在 Windows 下编译失败；Xray-core 原生支持 HTTPUpgrade / xhttp / Reality

### 组件更新策略
每周一自动编译，以下组件每次拉取最新版：

| 组件 | 来源 |
|------|------|
| xray-core | [XTLS/Xray-core](https://github.com/XTLS/Xray-core) latest |
| ck-client | [cbeuw/Cloak](https://github.com/cbeuw/Cloak) latest |
| v2ray-plugin | [teddysun/v2ray-plugin](https://github.com/teddysun/v2ray-plugin) latest |
| shadowsocks-rust | [shadowsocks/shadowsocks-rust](https://github.com/shadowsocks/shadowsocks-rust) latest |

以下组件锁定版本（与 `tun2socks.bin` API 强耦合，不可单独升级）：

| 组件 | 版本 | 原因 |
|------|------|------|
| tun2socks.bin | Netch 1.9.7 | 仓库内置，闭源 |
| aiodns.bin | Netch 1.9.7 | 配套版本 |
| wintun.dll | 0.13 | tun2socks.bin 编译时绑定此版本，换 0.14 会崩溃 |

## 下载

前往 [Actions](../../actions/workflows/build-httpupgrade.yml) 页面，点击最新一次成功的 workflow run，在底部 Artifacts 下载 `Netch-httpupgrade-xxx.zip`。

## 使用说明

### HTTPUpgrade / xhttp
在添加 VLESS 或 VMess 服务器时，传输协议下拉框选择 `httpupgrade` 或 `xhttp`，Host 和 Path 字段同样生效。

### Reality
在添加 VLESS 服务器时：
1. 传输协议选择 `tcp`
2. TLS Secure 选择 `reality`
3. 填写 **Public Key**、**Short ID**、**Fingerprint**（默认 `chrome`）
4. SNI 填写 serverName

## 模式说明

| 模式 | 说明 |
|------|------|
| ProcessMode | 使用 Netfilter 驱动拦截指定进程流量 |
| TunMode | 使用 WinTUN 虚拟网卡，全局代理 |
| ShareMode | 基于 WinPcap/Npcap 共享网络 |
| WebMode | Web 代理模式 |

## License

Netch is licensed under the [GPLv3](https://raw.githubusercontent.com/netchx/netch/main/LICENSE) license
