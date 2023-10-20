---
title: "Restic"
subtitle: ""
date: 2023-10-20T23:24:57+08:00
lastmod: 2023-10-20T23:24:57+08:00
draft: false
authors: ["zhxqian3"]
summary: "一个好用的备份工具"
tags: ["update","tools","notes"]
featuredImage: ""
hiddenFromHomePage: false
hiddenFromSearch: false
toc: 
  enable: true
---

## 安装
```
sudo pacman -S restic
```

## 使用

### 创建存储库（本地）
```
restic init --repo /srv/restic-repo
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
restic -r /srv/restic-repo key list
```
```
restic -r /srv/restic-repo key add
```
