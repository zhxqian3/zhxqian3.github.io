+++
title = '配置无下限'
date = 2023-09-07T15:38:00+08:00
draft = false
+++

## 何为无下限？
“无下限”是《咒术回战》中五条悟的术式。这篇post讲述的是对一些代理工具的大概配置。为什么要用“无下限”来称呼这些代理？在我看来，这些代理工具从某种意义做到了无下限的“不可侵”。本人是从22年2、3月左右接触到了像v2ray、xray那样的代理软件，刚开始是什么也不懂，摸索的时候也曾误入使用那些免费vpn软件的错误道路，直到现在也不能说在这方面完全入门了，所以本文仅对一些代理工具提供一些大概的配置，具体的配置还需要去到对应的项目自行甄别，仅供参考。另外，需要注意这篇文章的写作日期，如果日期太过久远，可能这篇文章已经过时。
## v2ray v5格式配置
v2ray从它的5.*版本以上就可以使用v5格式的配置。在我看来，v5格式比v4格式要简洁许多，虽然官方文档对于一些人来说可能“过于简洁”，但是如果一步一步按照官方文档说的格式去配置，是可以成功配置出来的。另外，要注意，如果使用v5格式的话，v2ray启动命令需要使用：
```
/v2ray run -c $configure_file_name -format jsonv5
```
对于配置文件，我只给出在服务端的大概配置，示例协议为`Vmess+grpc`，如下：
```
{
    "log": {
        "access": {
            "type": "File",
            "path": "string",
            "level": "Info"
        },
        "error": {
            "type": "File",
            "path": "string",
            "level": "Error"
        }
    },
    "router": {
        "domainStrategy": "IpIfNonMatch",
        "rule": [
            {
                "tag": "block",
                "geoDomain": [
                    {
                        "code": "cn",
                        "filePath": "string"
                    }
                ]
            },
            {
                "tag": "block",
                "geoip": [
                    {
                        "inverseMatch": false,
                        "code": "cn",
                        "filePath": "string"
                    }
                ]
            }
        ]
    },
    "inbounds": [
        {
            "protocol":"vmess",
            "settings":{
                "users": ["UID"]
            },
            "port":"",
            "listen":"127.0.0.1",
            "tag":"grpc-in",
            "sniffing":{
                "enabled": true
            },
            "streamSettings":{
                "transport": "grpc",
                "transportSettings": {
                    "serviceName": "string"
                }
            }
        },
        {
            "protocol":"socks",
            "settings":{
                "udpEnabled": true
            },
            "port":"",
            "listen":"127.0.0.1",
            "tag":"socks-in",
            "sniffing":{
                "enabled": true
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom",
            "tag": "direct"
        },
        {
            "protocol": "blackhole",
            "tag": "block"
        }
    ]
}
```
具体配置时还是建议去官方文档上面查看。对于xray的配置，xray-core有一个`xray-examples`的项目，更新较为频繁，去上面查找即可。
## 其他
对于其他的一些代理工具，例如sing-box、tuic、hysteria、naiveproxy等，我认为按照官方的说明也能够配置的出来，这些也都是特别好用的双端代理工具。当然，还有clash.meta、dae、v2rayA等一些十分好用的客户端代理工具。如果觉得这些工具好用的话，可以给上面那些项目点个star，有能力的话可以考虑贡献一份力。对于我这个门外人来说，就只能给个star了，www。
## 可以参考
- [xray-examples](https://github.com/XTLS/Xray-examples)
- [xray-docs](https://xtls.github.io/)
- [v2fly-docs](https://www.v2fly.org/)
- [sing-box-docs](https://sing-box.sagernet.org/)
- [dae](https://github.com/daeuniverse/dae)