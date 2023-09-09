+++
title = '使用certbot申请ssl证书'
date = 2023-09-09T15:48:01+08:00
draft = false
tags = ["notes","update"]
searchHidden = false
showToc = true
author = ["zhxqian3"]
summary = "。。"
+++

## 安装 snap

```
sudo apt update
sudo apt install snapd
```

注销并重新登录，或重新启动系统，以确保正确更新 snap 的路径。

```
reboot
```

在此之后，安装coresnap 以获得最新的snapd

```
sudo snap install core
```

显示：
core 16-2.45.2 from Canonical✓ installed

* * *

要测试您的系统，请安装hello-world snap 并确保它正确运行

```
sudo snap install hello-world
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
sudo apt-get remove certbot
```

## 安装 Certbot

```
sudo snap install --classic certbot
```

## 准备 Certbot 命令

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## 获取并安装证书

```
sudo certbot certonly --standalone
```

## 测试自动续订

```
sudo certbot renew --dry-run
```
续订 certbot 的命令安装在以下位置之一：
- `/etc/crontab/`
- `/etc/cron.*/*`
- `systemctl list-timers`

---
Last update:2023-9-9