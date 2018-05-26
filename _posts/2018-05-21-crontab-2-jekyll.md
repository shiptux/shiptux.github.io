---
layout: post
title: "使用cron定时推送jekyll博客"
author: shiptux
date: 2018-05-22 22:20:20 +800
---

0. 使用定时服务可以大大减少重复性的劳动工作，有效解放劳动力

1. 首先我在  <code>/home/ubuntu/script/</code> 目录新建一个简单 `shell` 文件记录一些关于推送的操作

2. 在 `Ubuntu` 下使用来进行 `Crontab` 定时调度该脚本

```bash
crontab -u 指定用户 -l 列出任务计划 -r 删除任务计划 -e编辑任务
```

- 直接编辑 <code>/etc/crontab</code> 或者使用<code>crontab -e</code> 命令编辑调度任务

第一次执行 <code>crontab -e</code> 会提示选择编辑器

```bash
ubuntu@ubuntu1:~$ sudo crontab -e
no crontab for root - using an empty one

Select an editor.  To change later, run 'select-editor'.
  1. /bin/nano        <---- easiest
  2. /usr/bin/vim.basic
  3. /usr/bin/vim.tiny
  4. /usr/bin/emacs25
  5. /usr/bin/code
  6. /bin/ed

Choose 1-6 [1]: 5
You are trying to start vscode as a super user which is not recommended. If you really want to, you must specify an alternate user data directory using the --user-data-dir argument.
crontab: "/usr/bin/sensible-editor" exited with status 1
```

3. Cron格式

分    小时   日     月    星期  命令
0-59  0-23  1-31  1-12  0-6   命令或者脚本路径
特殊符号：

"*":可以填充不指定的字段

"/"：“每”

"-": 从某个数到另一个数字

","：分开几个离散的数字

4. 启动服务

查看服务是否运行

<code>ps -ax | grep cron</code>

使用 <code>sudo service cron start</code> 启动