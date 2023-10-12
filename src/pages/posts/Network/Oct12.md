---
layout: "../../../layouts/MarkdownPost.astro"
title: localhost 引发的惨案
pubDate: 2023-10-12
# description: 着俩居然有区别？
author: "cos"
cover:
  url: "https://pic.imgdb.cn/item/65071a1e661c6c8e546b59b6.jpg"
  square: "https://pic.imgdb.cn/item/65071a1e661c6c8e546b59b6.jpg"
alt: "cover"
tags: ["Network"]
featured: true
theme: 'light'
---

 今天使用 `TypeOrm` 连接本地 mysql ，居然报错了。

![| w](https://pic.imgdb.cn/item/6527acffc458853aefcd088b.jpg)





用 navicat 可以正常连接，说明服务已经成功启动，反复检查 typeorm 配置文件，并不觉得有什么问题。

于是翻了一下互联网，并没有找到解决方案。

疑惑，大大的疑惑。

我甚至使用了重启大法，以为是本地网络环境出了点问题，然而重启大法也失效了。



于是我死马当作活马医式的，将配置文件中的 `localhost` 改为了 `127.0.0.1`，毫无期待的输入启动命令，结果，我嘴巴张大了，居然启动成功了。



## Why ?

于是我陷入了沉思，究竟是什么原因产生了这样的区别？

显然，`localhost` 与 `127.0.01` 的区别是，一个是**域名**，一个是**ip地址**。

域名需要解析为 ip地址 才能使用，因此，是在转换的这个过程出现了问题。

于是我再次仔细的查看报错信息：

```bash
[Nest] 2554  - 10/12/2023, 3:56:41 PM   
ERROR [TypeOrmModule] Unable to connect to the database. Retrying (1)...
Error: connect ECONNREFUSED ::1:3306
```

注意最后一行 `::1:3306`.

经过查询，原来这是 **ipv6 地址** 的写法。

在域名转换为ip地址的时候，目前有两种协议，分别是 ipv4 与 ipv6。

- IPv4 地址由四个 0 到 255 之间的数构成，用点（.）分隔，例如：﻿192.0.2.0。
- IPv6 地址由八组四位十六进制数构成，用冒号（:）分隔，例如：﻿2001:0db8:85a3:0000:0000:8a2e:0370:7334。

`localhost` 这个域名，通过 ipv4 协议解析的结果是 `127.0.0.1`,  通过 ipv6 协议解析的结果是 `::1`. 



我本地的 mysql 服务，只监听了 ipv4 协议，但我的主机却默认将 `localhost` 通过 ipv6 解析为了 `::1` 。因此连接 mysql 失败。

当我直接指定 `127.0.0.1` ，相当于强制使用了 ipv4 协议，于是连接成功。



通过这个小小的 bug，我意识到了，localhost 域名使用需谨慎，脑子里要有两种解析协议 ipv4 与 ipv6 的概念，算是我之前完全遗漏的知识。



狠狠恶补计网这件事，看来需要尽早了啊～
