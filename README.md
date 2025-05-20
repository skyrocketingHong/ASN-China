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
# ASN-China
Real-time updated Chinese ASN and IP database.

## Features
- Automatic daily updates
- Reliable and accurate source

## Use in proxy app
### Mihomo
```yaml
rule-providers:
    cnasn:
        type: http
        behavior: classical
        format: text
        path: ./rules/cnasn.list
        url: "https://raw.githubusercontent.com/skyrocketingHong/ASN-China/main/ASN.China.list"
        interval: 86400
rules:
    - RULE-SET,cnasn,DIRECT
```

## Data Source
### ASN Information
- [bgp.he.net](https://bgp.he.net/country/CN)

### IP Information
- [cbuijs/ipasn](https://github.com/cbuijs/ipasn)

## Author's ASN
**[AS206729](https://bgp.he.net/AS206729)**

The ASN name has been officially changed in the Jan 20, 2022 UTC [Commit](https://github.com/missuo/ASN-China/commit/4345acd8e146c99d56792977d88ed1d6417c9e22).

## Author

**ASN-China** © [Vincent Young](https://github.com/missuo), Released under the [MIT](./LICENSE) License.<br>
