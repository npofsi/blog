﻿---
title: 查询 MAC 地址
published: 2018-05-17 12:57:28 +08:00
description: 使用 OUI 查询 MAC 地址
category: Tip
tags: [Network, Code, Standard]
draft: false
---


MAC 是 IEEE 规定的国际网卡标识号码，只有获得 MAC 地址字段的厂家才可以生产网卡。
这个字段是公开的（即号码的前六位）储存在一个 oui.txt 文件里， IEEE 内部是用 PHP 解析的，这个文件标注了字段所属公司的名称，地址等信息.如果需要可以访问：
	[oui.txt](http://standards-oui.ieee.org/oui/oui.txt)