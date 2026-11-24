<p align="center">
  <a href="README.md">English</a> | 简体中文
</p>

<h1 align="center">ASN-China</h1>

<p align="center">纯文本路由规则集，收录中国和美国的 ASN，以及中国的 IPv4 和 IPv6 网段。</p>

<p align="center">
  <a href="https://github.com/skyrocketingHong/ASN-China/actions/workflows/ci.yml"><img src="https://github.com/skyrocketingHong/ASN-China/actions/workflows/ci.yml/badge.svg?branch=main" alt="ASN 和 IP 清单更新状态"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue" alt="MIT 许可证"></a>
</p>

## 规则文件

可通过原始链接下载规则文件，也可将链接填入客户端的远程规则集配置。

| 文件 | 条目 | 原始链接 |
| --- | --- | --- |
| [`ASN.China.list`](ASN.China.list) | 中国 ASN 规则（`IP-ASN,<编号>`） | [原始文件](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/ASN.China.list) |
| [`ASN.US.list`](ASN.US.list) | 美国 ASN 规则（`IP-ASN,<编号>`） | [原始文件](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/ASN.US.list) |
| [`IP.China.list`](IP.China.list) | 中国 IPv4 和 IPv6 网段（CIDR） | [原始文件](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IP.China.list) |
| [`IPv4.China.list`](IPv4.China.list) | 中国 IPv4 网段（CIDR） | [原始文件](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IPv4.China.list) |
| [`IPv6.China.list`](IPv6.China.list) | 中国 IPv6 网段（CIDR） | [原始文件](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IPv6.China.list) |

## 在 Mihomo 中使用

此示例将匹配中国 ASN 或 IP 网段的连接设为直连（`DIRECT`）。将对应条目合并到已有的 `rule-providers` 和 `rules` 配置中，并把这两条规则放在兜底的 `MATCH` 规则之前。

```yaml
rule-providers:
  cnasn:
    type: http
    behavior: classical
    format: text
    path: ./rules/cnasn.list
    url: https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/ASN.China.list
    interval: 86400
  cnip:
    type: http
    behavior: ipcidr
    format: text
    path: ./rules/cnip.list
    url: https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IP.China.list
    interval: 86400
rules:
  - RULE-SET,cnasn,DIRECT
  - RULE-SET,cnip,DIRECT
```

切换文件时保留 `format: text`，ASN 规则选用 `behavior: classical`，CIDR 清单选用 `behavior: ipcidr`。示例每隔 86,400 秒（24 小时）检查更新。[Mihomo 规则集文档](https://wiki.metacubex.one/en/config/rule-providers/)

## 数据来源与更新

- `scripts/ASN_CN.py` 和 `scripts/ASN_US.py` 从 Hurricane Electric BGP Toolkit 的国家页面提取 ASN 并生成规则。[中国](https://bgp.he.net/country/CN) · [美国](https://bgp.he.net/country/US)
- `scripts/IP_CN.py` 同步上游的中国 CIDR 文件，包括混合网段、IPv4 和 IPv6 三份清单。[cbuijs/ipasn](https://github.com/cbuijs/ipasn)

GitHub Actions 计划每天北京时间 00:00（UTC 16:00）刷新数据，也会在推送或手动触发时运行，并将生成的数据改动提交到仓库。[工作流运行记录](https://github.com/skyrocketingHong/ASN-China/actions/workflows/ci.yml)

## 本地生成

使用与工作流一致的 Python 3.13 环境，在仓库根目录执行以下命令。脚本会下载上游数据并覆盖五份清单。

```bash
python -m pip install requests lxml
python scripts/IP_CN.py
python scripts/ASN_CN.py
python scripts/ASN_US.py
```

## 致谢与许可证

ASN-China 原项目由 Vincent Young 创建。[作者](https://github.com/missuo) · [上游仓库](https://github.com/missuo/ASN-China)

上游项目记录了作者的 ASN 及历史名称变更。[AS206729](https://bgp.he.net/AS206729) · [上游提交](https://github.com/missuo/ASN-China/commit/4345acd8e146c99d56792977d88ed1d6417c9e22)

项目代码采用 [MIT 许可证](LICENSE)。

## AI 辅助开发

本项目在开发过程中使用生成式 AI 协助编码。

[![Vibe PR](https://raw.githubusercontent.com/fenxer/llm-things/main/stickers/vibe-pr.svg)](https://github.com/fenxer/llm-things/blob/main/stickers/vibe-pr.svg)
