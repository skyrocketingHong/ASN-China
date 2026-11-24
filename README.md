<!--
 * @Author: Vincent Young
 * @Date: 2022-11-17 02:07:33
 * @LastEditors: Vincent Young
 * @LastEditTime: 2022-11-17 03:33:16
 * @FilePath: /ASN-China/README.md
 * @Telegram: https://t.me/missuo
 *
 * Copyright © 2022 by Vincent, All Rights Reserved.
-->

<p align="center">
  English | <a href="README_ZH.md">简体中文</a>
</p>

<h1 align="center">ASN-China</h1>

<p align="center">Plain-text routing lists for China and US ASNs, plus China IPv4 and IPv6 prefixes.</p>

<p align="center">
  <a href="https://github.com/skyrocketingHong/ASN-China/actions/workflows/ci.yml"><img src="https://github.com/skyrocketingHong/ASN-China/actions/workflows/ci.yml/badge.svg?branch=main" alt="ASN and IP list update status"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue" alt="MIT license"></a>
</p>

## Rule files

Use the raw URLs to download the lists or configure a remote rule provider.

| File | Entries | Raw URL |
| --- | --- | --- |
| [`ASN.China.list`](ASN.China.list) | China ASN rules (`IP-ASN,<number>`) | [Raw](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/ASN.China.list) |
| [`ASN.US.list`](ASN.US.list) | US ASN rules (`IP-ASN,<number>`) | [Raw](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/ASN.US.list) |
| [`IP.China.list`](IP.China.list) | China IPv4 and IPv6 CIDRs | [Raw](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IP.China.list) |
| [`IPv4.China.list`](IPv4.China.list) | China IPv4 CIDRs | [Raw](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IPv4.China.list) |
| [`IPv6.China.list`](IPv6.China.list) | China IPv6 CIDRs | [Raw](https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/IPv6.China.list) |

## Use in Mihomo

This example sends connections matching the China ASN or CIDR lists to `DIRECT`. Merge the entries into your existing `rule-providers` and `rules` sections, and place them before the catch-all `MATCH` rule.

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

Keep `format: text` when switching lists. Use `behavior: classical` for either ASN list and `behavior: ipcidr` for any CIDR list. The example checks for updates every 86,400 seconds (24 hours). [Mihomo rule-provider documentation](https://wiki.metacubex.one/en/config/rule-providers/)

## Data sources and updates

- `scripts/ASN_CN.py` and `scripts/ASN_US.py` generate the ASN rules from Hurricane Electric BGP Toolkit's country pages. [China](https://bgp.he.net/country/CN) · [United States](https://bgp.he.net/country/US)
- `scripts/IP_CN.py` copies the upstream China CIDR lists for both address families and for each family individually. [cbuijs/ipasn](https://github.com/cbuijs/ipasn)

GitHub Actions schedules a daily refresh at 16:00 UTC (00:00 China Standard Time). The workflow also runs on pushes and manual dispatch and commits generated changes to the repository. [Workflow runs](https://github.com/skyrocketingHong/ASN-China/actions/workflows/ci.yml)

## Regenerate locally

Use a Python 3.13 environment, matching the workflow. Run the commands from the repository root. The scripts fetch upstream data and overwrite the five list files.

```bash
python -m pip install requests lxml
python scripts/IP_CN.py
python scripts/ASN_CN.py
python scripts/ASN_US.py
```

## Credits and license

Vincent Young created the original ASN-China project. [Author](https://github.com/missuo) · [Upstream repository](https://github.com/missuo/ASN-China)

The upstream project records its author's ASN and the historical name change. [AS206729](https://bgp.he.net/AS206729) · [Upstream commit](https://github.com/missuo/ASN-China/commit/4345acd8e146c99d56792977d88ed1d6417c9e22)

The project code is available under the [MIT License](LICENSE).

## AI-assisted development

This project was developed with assistance from generative AI.

[![Vibe PR](https://raw.githubusercontent.com/fenxer/llm-things/main/stickers/vibe-pr.svg)](https://github.com/fenxer/llm-things/blob/main/stickers/vibe-pr.svg)
