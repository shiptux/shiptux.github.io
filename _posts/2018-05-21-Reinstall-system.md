---
layout: post
date: 2018-05-21 20:31:01 +800
author: moryu
title: "给云主机重装系统"
---

在购买一些主机商的服务后，有时他们提供的镜像中带了一些卸载不掉的东西就像是买了手机预装了软件一样，这时候我们选择手动安装纯净版。

```
wget --no-check-certificate -qO DebianNET.sh "http://status.moruy.cn/DebianNET.sh" && chmod a+x DebianNET.sh
```

`Centos` 先要安装 `gawk` `sed` `grep`

<code>sudo yum install -y gawk sed grep < /code>

切换到超级用户

<code>sudo -i < /code>

安装 `Ubuntu 16.04 x64` :
```
bash DebianNET.sh -u xenial -v 64
```

安装 `Debian 8 x64` :
```
bash DebianNET.sh -d jessie -v amd64
bash DEbianNET.sh -d 8 -v 64
```

第一步选择 `n`