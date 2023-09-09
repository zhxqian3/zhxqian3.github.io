+++
title = '在debian上安装docker'
date = 2023-09-09T15:33:00+08:00
draft = false
tags = ["notes"]
searchHidden = false
showToc = true
author = ["zhxqian3"]
summary = "如题。"
+++

## 使用apt安装

### 设置存储库
若安装过，先卸载旧版本:
```
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

更新apt包索引并安装包以允许apt通过 HTTPS 使用存储库：

```
sudo apt-get update
```

```
sudo apt-get install ca-certificates curl gnupg
```

添加 Docker 的官方 GPG 密钥：

```
sudo install -m 0755 -d /etc/apt/keyrings
```

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

使用以下命令设置存储库：

```
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 安装

更新apt包索引，安装最新版本的 Docker Engine、containerd 和 Docker Compose，或者进入下一步安装特定版本：

```
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 升级 Docker 引擎

```
sudo apt update
```

## 卸载

```
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

```
sudo rm -rf /var/lib/docker
```

```
sudo rm -rf /var/lib/containerd
```

[相关网址](https://docs.docker.com/engine/install/debian/)