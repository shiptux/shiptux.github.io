---
layout: post
title: "建议在课堂使用Github"
author: licheng
date: 2018-09-11 08:02:00
categories: tutorial
---

也许你听说并使用过 `Git` ，也许你第一次听说。 `Git` 是 `Linux` 下一个很棒的工具， `Github` 是一个面向开源 

及私有软件项目的托管平台，支持版本控制，协作，代码片段分享。

`Github` 有很多开源组织，很多免费使用的优质资源。有很多知名或是强大的项目托管于 `Github`，比如 `Linux` 系统

![kernel](/assets/2018/09/11/kernel.png)

内核项目。掌握 `Github` 的使用有助于与国际接轨，学习先进知识。并且现在的部分公司面试时候 要求会使用 `Github` 

因此我建议我们的课程内容中加入 `Github` 相关内容，让同学们注册帐号，并使用该方式管理作业。

这里用 `Github` 怎么完成第一次的作业

## 准备工作

0. 如果你的电脑上还没有安装 `Git` ，请先执行 `sudo dnf install git -y` 或者  `sudo apt install git -y` 安装

1. 注册 GitHub 帐号

    <https://github.com/join>

2. 新建一个仓库

    ![Repository](/assets/2018/09/11/Repository.png)

    ![Repository1](/assets/2018/09/11/Repository1.png)

    ![Repository2](/assets/2018/09/11/Repository.png2)



3. 克隆仓库到本地

    ![Clone](/assets/2018/09/11/Clone.png)

4. 在终端中输入若干行命令并截图 我在 `Ubuntu` 的设置里将截图设置为了 `Alt+Ctrl+A`

    ![Code](/assets/2018/09/11/Code.png)

5. 复制图片文件到仓库目录 并提交

    ![Push](/assets/2018/09/11/Push.png)

```bash
#添加所有文件，注意后面那个点

git add . 

#将文件提交到本地仓库

git commit -am "提交作业"

#提交

git push origin master

```

还有 `Pull request` 请求合并等方式 敬请你的探索，这里只做简单介绍。