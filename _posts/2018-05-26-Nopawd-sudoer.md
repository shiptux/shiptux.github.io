---
layout: post
title: "补充关于sudoer无密码的设置"
author: shiptux
date: 2018-05-26 14:12:22 +800
---

0. `sudo` 命令是我的一个常用命令,可以让普通用户借用超级用户的权限。以往我在 `Fedora` 下的时候，往往 `sudo` 用户的密码设置是为空的，因为本身不需要作太多安全性考虑怎么方便怎么来。但是在部分发行版下禁止密码为空。

1. 参考网上资料。切换到 `root` 用户执行 <code>visudo</code> 或者用 `sudo` 用户执行 <code>sudo visudo</code> 。这里不建议直接编辑`sudoer` 文件

```
# User privilege specification
root    ALL=(ALL:ALL) ALL
ubuntu  ALL=(ALL:ALL) ALL
# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

一般教程往往建议在 <code>root    ALL=(ALL:ALL) ALL</code> 下增添或者修改为 <code>ubuntu  ALL=(ALL:ALL) NOPASSWD:ALL</code> 但是我在实际使用中发现这样还是需要每次输入密码，而且系统下已经有 `sudo` 用户组权限的设置 我将最后一行 <code>sudo ALL=(ALL:ALL) ALL</code> 改为 <code>sudo ALL=(ALL:ALL) NOPASSWD:ALL</code>