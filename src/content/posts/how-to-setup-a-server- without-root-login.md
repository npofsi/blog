---
title: 不使用 root 账户登陆服务器
published: 2022-06-25 9:26:02 +08:00
category: Note
tags: [Server, Maintenance, SSH]
description: ''
image: ./cover.jpg
draft: false
# lang: jp      # 仅当文章语言与 `config.ts` 中的网站语言不同时需要设置
---

之前服务器的时候遇到疑似 root 被爆破的情况，所幸最后查明是服务商流量计费出错了，而且使用 root 登陆服务器权限过大太过危险，容易发生 `rm -rf` 这种事故，所以这次建服的时候就考虑了一下这些事情，而且操作起来并不是很麻烦。

#### 基本信息

一般云服务提供商会提供 root 账户的密码，这里用 `[password]` 表示
<!-- more -->
#### 新建用户

首先使用 root 账户登录服务器

```shell
PS> ssh root@[server]
```

添加一个新的账户

```shell
root> useradd [username] -m
```

#### 赋予该账户 sudo 权限

编辑配置文件

```shell
root> visudo
```

在 root 账户权限旁加入一行

```shell
[username] ALL=(ALL:ALL) ALL
```

#### 编辑SSH配置文件

```shell
root> vim /etc/ssh/sshd_config
```

在文件中添加新的一行

```shell
AllowUsers [username1] [username2]
```

并修改 root 是否可以登录 ssh

```shell
PermitRootLogin no
```

!!! Warning 请确保以上编辑工作无误后继续

保存文件后，运行

```shell
root> service sshd restart
```

现在该服务器就不可以使用 root 账户登录 ssh 了，需要使用新的账户来登陆服务器。

#### 使用SSHKey登录

使用 ssh-copy-id 将 sshkey 公钥导入服务器，公钥储存在 ~/.ssh/authorized_keys

```shell
PS> ssh-copy-id [username]@[server]
```
