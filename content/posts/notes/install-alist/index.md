---
title: "Alist"
date: 2024-01-10T14:35:10+08:00
lastmod: 2024-01-10T14:35:10+08:00
draft: false
authors: ["zhxqian3"]
summary: "在linux上的alist的安装"
tags: ["notes","update"]
hiddenFromHomePage: false
hiddenFromSearch: false
toc: 
  enable: true
---

## 手动安装
***注意***：手动安装本人没试过，可能有误。

### 获取 AList
从[releases](https://github.com/alist-org/alist/releases)上下载相应的文件，当你看到`start server@0.0.0.0:5244`的输出，之后没有报错，说明操作成功。 第一次运行时会输出初始密码。程序默认监听`5244`端口。 现在打开`http://ip:5244`可以看到登录页面。

### 手动运行
v3.25.0以上版本将密码改成加密方式存储的hash值，无法直接反算出密码，如果忘记了密码只能通过重新`随机生成`或者`手动设置`。
```
# 解压下载的文件，得到可执行文件：
tar -zxvf alist-xxxx.tar.gz
# 授予程序执行权限：
chmod +x alist
# 运行程序
./alist server

# 获得管理员信息 以下两个不同版本，新版本也有随机生成和手动设置
# 低于v3.25.0版本
./alist admin

# 高于v3.25.0版本
# 随机生成一个密码
./alist admin random
# 手动设置一个密码 `NEW_PASSWORD`是指你需要设置的密码
./alist admin set NEW_PASSWORD
```

### 守护进程
使用任意方式编辑`/etc/systemd/system/alist.service`并添加如下内容，其中`path_alist`为`AList`所在的路径。
```
[Unit]
Description=alist
After=network.target
 
[Service]
Type=simple
WorkingDirectory=path_alist
ExecStart=path_alist/alist server
Restart=on-failure
 
[Install]
WantedBy=multi-user.target
```

***tips***:对于所有平台，您可以使用以下命令来静默启动、停止和重新启动。 （v3.4.0 及更高版本）
```
# 携带`--force-bin-dir`参数启动服务
alist start
# 通过pid停止服务
alist stop
# 通过pid重启服务
alist restart
```

### 如何更新
下载新版Alist，把之前的替换了即可。

### 反向代理
配置文件`config.json`路径：`/xx 路径/data/config.json`。

#### 配置`site_url`
你的网站 URL，比如`https://pan.nn.ci`，这个地址会在程序中的某些地方使用，如果不设置这个字段，一些功能可能无法正常工作，另外URL链接结尾请勿携带`/`。

#### 配置`scheme`
填写示例：记得把证书文件丢到`data`目录里面才会识别到喔~
```
  "scheme": {
    "address": "0.0.0.0",   // 要监听的 http/https 地址，默认为 0.0.0.0
    "http_port": 5244,      // 监听的 http 端口,默认的 '5244',如果你想禁用 http,将其设置为 '-1'
    "https_port": -1,       // https 端口监听,默认的 '-1',如果你想启用 https,将其设置为非 '-1'
    "force_https": false,   // 是否强制使用 HTTPS 协议,如果设置为 true ,则用户只能通过 HTTPS 访问该网站
    "cert_file": "data\\cert.crt",  // 证书文件路径
    "key_file": "data\\key.key",    // 证书密钥文件路径
    "unix_file": "",         // Unix 监听套接字文件路径,默认的空的,如果你想使用 Unix socket,将其设置为非空
    "unix_file_perm": ""     // Unix 监听套接字文件，设置为合适的权限
  },
```

## docker
docker compose:
```
version: '3.3'
services:
    alist:
        image: 'xhofe/alist:latest'
        container_name: alist
        volumes:
            - '/etc/alist:/opt/alist/data'
        ports:
            - '5244:5244'
        environment:
            - PUID=0
            - PGID=0
            - UMASK=022
        restart: unless-stopped
```

## 复制来源
[alist官网](https://alist.nn.ci/)