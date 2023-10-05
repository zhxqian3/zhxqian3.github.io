---
title: '在debian上开启bbr'
date: 2023-09-09T15:38:16+08:00
draft: false
tags: ["notes"]
authors: ["zhxqian3"]
summary: "。。。"
---

Debian10 / 11 默认的内核就是 4.19 版本的内核而且编译了 TCP BBR 模块，所以可以直接通过参数开启。
```
echo net.core.default_qdisc=fq >> /etc/sysctl.conf
```
```
echo net.ipv4.tcp_congestion_control=bbr >> /etc/sysctl.conf
```
```
sysctl -p
```
检查是否成功：
```
sysctl net.ipv4.tcp_available_congestion_control
```
显示：
```
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```
表示开启成功。

**或执行 lsmod | grep bbr ，检测 BBR 是否开启。**
```
lsmod | grep bbr
```
```
lsmod | grep fq 
```
返回tcp_bbr和sch_fq

重启
```
reboot
```
查看版本内核
```
uname -r  
```
-v 　显示操作系统的版本