---
title: 使用 ssh 访问 Github （从创建密钥到 clone 仓库）
published: 2023-05-22 10:12:28 +08:00
category: Tip
tags: [git, Github, ssh]
draft: false
# lang: jp      # 仅当文章语言与 `config.ts` 中的网站语言不同时需要设置
---


最近在打编译器设计赛，帮队友配置 git 的时候发现配置 ssh 的部分每次要看好几篇文章，现在用这一篇文章就可以在一台新的 Linux 系统上快速使用 ssh 访问 Github 进行开发。

## 创建 ssh 密钥

```sh
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```

如果不支持 ed25519，可以使用：

```sh
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

中间会需要选择密钥的创建位置等：

```text
> Enter a file in which to save the key (/c/Users/YOU/.ssh/id_ALGORITHM):[Press enter]
```

密钥默认生成在 `~/.ssh/id_ALGORITHM`,

`ALGORITHM` 即上面创建密钥时使用的算法

`~/.ssh/id_ALGORITHM.pub` 为公钥文件;

`~/.ssh/id_ALGORITHM` 为私钥文件;

## 添加 ssh 公钥到 GitHub

执行

```sh
$ cat ~/.ssh/id_ALGORITHM.pub
```

复制输出的公钥

在 [Github User Setting -> SSH and GPG keys](https://github.com/settings/keys) 页面添加 ssh 公钥。

## 启用 ssh agent

```sh
$ eval "$(ssh-agent -s)"
```

然后将我们的密钥交给 ssh-agent 管理

```sh
$ ssh-add ~/.ssh/id_ALGORITHM
```

在 `~/.ssh/` 目录下新建一个 config 文件

```sh
$ touch ~/.ssh/config
```

编辑加入以下内容：

```
HOST github.com
 User git
 ForwardAgent yes
```

*PS:*

如果直接通过 ssh 访问 github 有问题，可以通过下面的配置使用 443 端口进行访问：

```
Host gitee.com
	HostName ssh.gitee.com
	Port 443
	User git
	ForwardAgent yes
Host github.com
	HostName ssh.github.com
	Port 443
	User git
	ForwardAgent yes
```

## 测试

现在已经配置好了 ssh 可以用以下命令进行测试：

使用这个命令检查密钥是否被加入 ssh-agent

```sh
$ ssh-add -L
```

使用这个命令检查是否已经可以使用 ssh 访问 Github:

```sh
$ ssh -T git@github.com
```

如果以上命令返回了 success ，那么已经可以使用 ssh 访问 Github了！

## Clone 仓库

接着我们就能用下面的命令 clone 仓库了。

```sh
$ git clone <url>
```

refs：

[生成新的 SSH 密钥并将其添加到 ssh-agent](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

[使用 SSH 代理转发](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding)
