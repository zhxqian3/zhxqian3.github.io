---
title: '记录一下在git上的一些设置'
date: 2023-09-22T18:05:35+08:00
lastmod: 2023-10-06
draft: false
tags: ["notes","update"]
authors: ["zhxqian3"]
summary: "如题"
---

今天想调一下我电脑上的git的一些设置，主要是设置git的GPG签名以及通过ssh连接git的远程仓库，在其中遇到了一些设置的问题，所以写下这些以做记录。（注：我的电脑环境为Linux，其他操作系统的相关设置不怎么了解）。

## 设置GPG签名
主要参考来源：
- [github docs](https://docs.github.com/zh/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
- [arch linux wiki](https://wiki.archlinux.org/title/GnuPG#Configure_pinentry_to_use_the_correct_TTY)
- new bing

### 生成GPG密钥
```
gpg --full-generate-key
```
输入相关信息后就可以生成密钥了。

可以查看生成后的密钥：
```
gpg --list-secret-keys --keyid-format=long
```

可能会显示类似的输出：
```
$ gpg --list-secret-keys --keyid-format=long
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot <hubot@example.com>
ssb   4096R/4BB6D45482678BE3 2016-03-10
```

从 GPG 密钥列表中复制您想要使用的 GPG 密钥 ID 的长形式。 在本例中，GPG 密钥 ID 为 `3AA5C34371567BD2`，导出GPG密钥：
```
gpg --armor --export 3AA5C34371567BD2
```
复制以 `-----BEGIN PGP PUBLIC KEY BLOCK-----` 开头并以 `-----END PGP PUBLIC KEY BLOCK-----` 结尾的 GPG 密钥。

之后再按照github文档上的操作将密钥添加到账户中。

### git上设置GPG
我更喜欢在局部设置git，所以我习惯于设置git时不加`--global`选项，但下面我还是以github docs上面的命令为准。
```
git config --global user.signingkey 3AA5C34371567BD2
```
密钥ID以上面的ID为例。

```
git config --global commit.gpgsign true
```

另外，还需要在家目录下的`.bashrc`文件中添加一行：
```
export GPG_TTY=$(tty)
```
```
source .bashrc
```

至此，配置应该基本完成。

## 设置ssh连接远程仓库
主要参考来源：
- [github docs](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
- new bing

### 生成ssh密钥
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
输入一些信息以完成ssh密钥的生成。

之后再按照github文档上的操作将生成的公钥添加到github账户上。

### 关于ssh的一些本地设置
为了不显示未知指纹的警告，可以在家目录下的`.ssh/`文件夹中创建`known_hosts`文件，在这个文件中添加：
```
github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl
github.com ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEmKSENjQEezOmxkZMy7opKgwFB9nkt5YRrYMjNuG5N87uRgg6CLrbo5wAdT/y6v0mKV0U2w0WZ2YB/++Tpockg=
github.com ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCj7ndNxQowgcQnjshcLrqPEiiphnt+VTTvDP6mHBL9j1aNUkY4Ue1gvwnGLVlOhGeYrnZaMgRK6+PKCUXaDbC7qtbW8gIkhL7aGCsOr/C56SJMy/BCZfxd1nWzAOxSDPgVsmerOBYfNqltV9/hWCqBywINIR+5dIg6JTJ72pcEpEjcYgXkE2YEFXV1JHnsKgbLWNlhScqb2UmyRkQyytRLtL+38TGxkxCflmO+5Z8CSSNY7GidjMIZ7Q4zMjA2n1nGrlTDkzwDCsw+wqFPGQA179cnfGWOWRVruj16z6XyvxvjJwbz0wQZ75XK5tKSb7FNyeIEs4TT4jk+S4dhPeAUC5y+bDYirYgM4GC7uEnztnZyaVWQ7B381AK4Qdrwt51ZqExKbQpTUNn+EjqoTwvqNj4kqx5QUCI0ThS/YkOxJCXmPUWZbhjpCg56i+2aB6CmK2JGhn57K5mj0MNdBXA4/WnwH6XoPWJzK5Nyu2zB3nAZp+S5hpQs+p1vN1/wsjk=
```
[来源](https://docs.github.com/zh/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)

如果你的密钥文件不在常规文件夹，可以在家目录下的`.ssh/`文件夹中创建`config`文件，在文件中添加：
```
Host github.com
  User git
  IdentityFile ~/.ssh/id_rsa.github
  IdentitiesOnly yes
```
注意将`IdentityFile`后面的参数改为你的私钥所在路径。

至此，配置基本完成。

## 关于git子模块
这里主要是列举常用的git子模块命令。

***注意***: 如果远程仓库使用了git子模块，在克隆或拉取这个远程仓库时，git子模块并不会自动初始化或更新。

### git子模块添加
```
git submodule [--depth=1] add [submodule-repository-url] [path]
```

### git子模块初始化
```
git submodule init [path]
```
- `[path]`: 初始化特定子模块

### git子模块更新
```
git submodule update [--init] [--recursive] [--remote] [--merge]
```
- `--init`: 加入这个选项后就无需运行`git submodule init`
- `--recursive`: 告诉 Git 检查子模块中的嵌套子模块并更新它们
- `--remote`: 使用远程源的新内容更新
- `--merge`: 将子模块的上游更改与本地版本合并

### git子模块状态
```
git submodule status [--recursive]
```
该命令打印出 SHA-1 和每个子模块的路径。SHA-1 字符串可以具有三个不同的前缀:
- `-`前缀标记未初始化的子模块
- `+`标志表明签出的子模块提交与原始子模块存储库的状态不同
- `U`前缀会提醒合并冲突
- `--recursive`选项可在状态报告中包含嵌套子模块

### git子模块结束
```
git submodule deinit [path] [--force]
```
- `--force`: 取消初始化包含本地修改的子模块