---
title: '格式化写保护磁盘'
date: 2023-09-09T15:11:48+08:00
draft: false
tags: ["notes"]
authors: ["zhxqian3"]
summary: "在windows中格式化写保护磁盘"
toc: 
  enable: false
---

```
diskpart
```

```
list disk
```

```
select disk n
```

```
attributes disk clear readonly
```

```
clean
```

```
create partition primary
```

```
format fs=ntfs
```

```
exit
```