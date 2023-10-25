---
title: "Backup"
subtitle: ""
date: 2023-10-20T23:24:57+08:00
lastmod: 2023-10-25T23:24:57+08:00
draft: false
authors: ["zhxqian3"]
summary: "个人关于restic和rclone的使用，仅作记录"
tags: ["update","tools","notes"]
featuredImage: ""
hiddenFromHomePage: false
hiddenFromSearch: false
toc: 
  enable: true
---

## 安装
```
sudo pacman -S restic rclone
```

## restic

### 创建存储库
本地：
```
restic init -r /srv/restic-repo
```

rclone：
```
restic -r rclone:foo:bar init
```

### 备份
```
restic -r /srv/restic-repo --verbose backup ~/work [--tag foo --tag see]
```

### 使用存储库

#### 列出所有快照
```
restic -r /srv/restic-repo snapshots
```

#### 检查完整性和一致性
```
restic -r /srv/restic-repo check --read-data
```

### 恢复备份
```
restic -r /srv/restic-repo restore latest --target /tmp/restore-work
```
或
```
restic -r /srv/restic-repo mount /mnt/restic
```

### 删除备份快照
```
restic -r /srv/restic-repo forget bdbd3439
```
```
restic -r /srv/restic-repo prune
```
或
```
restic forget --keep-last 1 --prune
```

### 加密

#### 管理存储库密钥
```
restic key [list|add|remove|passwd] [ID] -r /srv/restic-repo
```

## rclone

### 配置
```
rclone config
```

#### google drive
https://rclone.org/drive/#making-your-own-client-id

### 双向同步
```
rclone bisync remote1:path1 remote2:path2 [-i --dry-run]
```
此命令同步规则：path1 -> path2，且此命令要求两边的路径都要存在

### 列出
```
rclone [ls|lsd|lsl]
```

### 删除
```
rclone purge remote:path [-i --dry-run]
```

### 创建路径
```
rclone mkdir remote:path
```