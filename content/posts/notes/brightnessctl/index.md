---
title: "Brightnessctl"
date: 2024-01-13T12:19:26+08:00
lastmod: 2024-01-13T12:19:26+08:00
draft: false
authors: ["zhxqian3"]
summary: "对亮度调节工具brightnessctl的个人使用记录"
tags: ["update","tools","notes"]
hiddenFromHomePage: false
hiddenFromSearch: false
toc: 
  enable: true
---

## 情况
虽然本人目前使用的桌面环境Gnome有亮度调节选项，但是默认的最低亮度好像是最大亮度的6%，一直保持这个亮度导致了我的视觉能力较以往有了较大的下降。虽然Gnome可以将亮度调到最大的1%，但重启电脑后亮度又会回到默认的6%，所以我决定使用`brightnessctl`这个亮度调节工具。

## 安装
```
sudo pacman -S brightnessctl
```

## 设置亮度
可以使用多种方式设置亮度，我使用的是绝对的百分比数值来设置亮度。
```
brightnessctl set 50%
```
以上命令将目前亮度设置到最大亮度的50%。

## 设置守护进程
为了每次开机都运行亮度调节命令，可以简单的将这个命令设置守护进程。
```
cd /etc/systemd/system/
```
```
sudo vim brightnessctl.service
```
将以下文字粘贴进去：
```
[Unit] 
Description=brightnessctl
[Service] 
Type=oneshot
ExecStart=brightnessctl set 1%
[Install] 
WantedBy=multi-user.target
```
设置开机自启：
```
sudo systemctl enable brightnessctl.service
```
到此设置完毕，希望我的眼睛视力不会再恶化。

## 来源
[brightnessctl manual pages](https://man.archlinux.org/man/extra/brightnessctl/brightnessctl.1.en)