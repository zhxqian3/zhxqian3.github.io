+++
title = 'Journalctl'
date = 2023-09-09T15:15:12+08:00
draft = false
tags = ["notes"]
searchHidden = false
showToc = true
author = ["zhxqian3"]
summary = "个人关于journalctl的使用笔记"
+++

systemd 提供了自己的日志系统（logging system），称为 journal。使用 systemd 日志，无需额外安装日志服务（syslog）。使用 journalctl(1) 命令读取日志。

Arch Linux 中，` /var/log/journal/` 目录是 systemd包 软件包的一部分。默认情况下 `/etc/systemd/journald.conf` 中的`Storage=` 为 `auto`，systemd 会将日志记录写入 `/run/systemd/journal`。若被删除，systemd 不会自动创建此目录，而是将日志写入 `/run/systemd/journal`，重启时内容会消失。如果 `journald.conf` 中的 `Storage=persistent`， `systemd-journald.service` 重启 或系统重启时会重新创建 `/var/log/journal/`。

## 过滤输出
- 显示本次启动后的所有日志：
```
journalctl -b
```
`journalctl -b -0` 显示本次启动的信息
`journalctl -b -1` 显示上次启动的信息
`journalctl -b -2` 显示上上次启动的信息
- 只显示错误、冲突和重要告警信息：
```
journalctl -p err..alert
```
- 显示特定进程的所有消息：
```
journalctl _PID=1
```
- 显示指定单元的所有消息：
```
journalctl -u man-db.service
```

## 日志大小限制
如果按上面的操作保留日志的话，默认日志最大限制为所在文件系统容量的 10%，即：如果 `/var/log/journal` 储存在 50GiB 的根分区中，那么日志最多存储 5GiB 数据。用 `systemd-journald` 日志查看当前设置:
```
journalctl -b -u systemd-journald
```
可以修改配置文件指定最大限制。如限制日志最大 50MiB：
```
/etc/systemd/journald.conf
+++++++++++++++++++++++++++
SystemMaxUse=50M
```
还可以通过配置片段而不是全局配置文件进行设置：
```
/etc/systemd/journald.conf.d/00-journal-size.conf
+++++++++++++++++++++++++++++++++++++++++++++++++
[Journal]
SystemMaxUse=50M
```
修改配置后要立即生效，请重启 systemd-journald.service 服务。

## 手动清理日志
`/var/log/journal `存放着日志, `rm `应该能工作. 或者使用`journalctl`,
例如:
- 清理日志使总大小小于 100M:
```
journalctl --vacuum-size=100M
```
- 清理最早两周前的日志：
```
journalctl --vacuum-time=2weeks
```