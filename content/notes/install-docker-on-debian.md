+++
title = '在debian上安装docker'
date = 2023-09-09T15:33:00+08:00
draft = false
tags = ["notes","update"]
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
设置 Docker 的 Apt 存储库：
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### 安装

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugins
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
sudo rm -rf /var/lib/containerd
```
您必须手动删除任何编辑的配置文件。

[相关网址](https://docs.docker.com/engine/install/debian/)

---
Last update:2023-9-9