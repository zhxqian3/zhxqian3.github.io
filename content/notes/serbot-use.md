+++
title = '使用certbot申请ssl证书'
date = 2023-09-09T15:48:01+08:00
draft = false
tags = ["notes"]
searchHidden = false
showToc = true
author = ["zhxqian3"]
summary = "。。"
+++

## 安装 snap

```
apt update
```

```
apt install python
```

```
apt install snapd
```

注销并重新登录，或重新启动系统，以确保正确更新 snap 的路径。

```
reboot
```

在此之后，安装coresnap 以获得最新的snapd

```
snap install core
```

显示：
core 16-2.45.2 from Canonical✓ installed

* * *

要测试您的系统，请安装hello-world snap 并确保它正确运行

```
snap install hello-world
```

显示：
hello-world 6.3 from Canonical✓ installed

```
hello-world
```

显示：
Hello World!

## 删除 certbot-auto 和任何 Certbot 操作系统包

```
apt-get remove certbot
```

## 安装 Certbot

```
snap install --classic certbot
```

## 准备 Certbot 命令

```
ln -s /snap/bin/certbot /usr/bin/certbot
```

## 获取并安装证书

```
certbot --nginx
```

或者获得证书

```
certbot certonly --nginx --register-unsafely-without-email
```

又或：

```
certbot certonly --standalone
```

## 测试自动续订

```
certbot renew --dry-run
```