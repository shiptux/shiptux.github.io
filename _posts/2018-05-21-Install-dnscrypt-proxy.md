---
layout: post
title: "安装 DNScrypt 和通过 SNAP 安装主题"
author: shiptux
date: 2018-05-22 13:34 +800
---

0. `DNScrypt` 是...我就不搬了，只要知道他能防止  `DNS` 污染避免很多小广告就对了

1. 在 `Ubuntu16.04` 下安装 `DNScrypt` 非常方便，这里安装的是服务端,在云主机上安装

<code> sudo apt-get install dnscrypt-proxy </code>

2. 启用服务

<code> dnscrypt-proxy --daemonize --local-address=127.0.0.3:53 --resover-name=cisco</code>

3. 将 `PC` 上的 `DNS` 设置为手动，将自己的主机地址设置为 `DNS` 地址

4. 在 `Ubuntu18.04` 下通过 `SNAP` 安装主题

<code>sudo sanp install communitheme --edge </code>