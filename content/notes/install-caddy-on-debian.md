+++
title = '在debian上安装caddy'
date = 2023-09-09T15:52:04+08:00
draft = false
tags = ["notes"]
searchHidden = false
showToc = true
author = ["zhxqian3"]
summary = "如题"
+++

[相关网址](https://caddyserver.com/docs/install#debian-ubuntu-raspbian)

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
```

```
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
```

```
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
```

```
sudo apt update
```

```
sudo apt install caddy
```