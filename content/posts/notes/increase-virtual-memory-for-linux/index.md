---
title: 'linux增加虚拟内存'
date: 2023-09-09T15:29:22+08:00
draft: false
tags: ["notes"]
authors: ["zhxqian3"]
summary: "如题。"
---

创建要作为swap分区的文件:增加512MB大小的交换分区，则命令写法如下，其中的count等于想要的块的数量（bs*count=文件大小）

```
dd if=/dev/zero of=/tmp/big_swap bs=1M count=512
```

目录空间大小

```
du -sh /tmp/big_swap
```

格式化为交换分区文件

```
chmod 0600 /tmp/big_swap
```

```
mkswap /tmp/big_swap
```

启用交换分区文件:

```
swapon /tmp/big_swap
```

使系统开机时自启用，在文件/etc/fstab中添加一行

```
/tmp/big_swap swap swap defaults 0 0 
```

重启

```
reboot
```

关闭某个分区

```
swapoff /tmp/big_swap
```