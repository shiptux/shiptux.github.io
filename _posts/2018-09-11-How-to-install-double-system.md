---
title: "如何安装双系统"
layout: post
author: licheng
date: 2018-09-11 08:02:00
categories: tutorial
---

本篇介绍如何安装双系统，以 `Windows` 与 `Linux` 为双系统。 `Linux` 发行版选用 `Ubuntu-18.04` 本篇假设你使用 

##准备工作

0. 原料：U 盘，个人电脑，你聪明的大脑。

1. 下载你所选用的发行版镜像 一般在官网下载，百度 "Ubuntu" 即可。

	<https://www.ubuntu.com/download/desktop>

	![Download](/assets/2018/09/11/Download.png)

2. 使用 U 盘刻录你的系统文件 在 `Windows` 下使用 UltraIso 或者其他类似软件刻录

3. 刻录完成后，假设你是 `Win10` ，请关闭你的快速启动，具体操作是点击电源->电源设置->选择电源按钮的功能->更改当前不可用的设置->关闭快速启动

	为你的 `Linux` 系统分区 使用 文件资源管理器->此电脑->管理->存储->磁盘服务->压缩卷或者删除一个卷 记得先把文件移走哦
	
	![Filemaneage](/assets/2018/09/11/Filemaneage.jpg)

	![Yasuo1](/assets/2018/09/11/Yasuo1.jpg)

	![Yasuo2](/assets/2018/09/11/Yasuo2.jpg)

	![Yasuo3](/assets/2018/09/11/Yasuo3.jpg)

4. 关闭 Windows 快速启动 

	在右下角电源图标按右键

	![Battle](/assets/2018/09/11/Battle.jpg)

	选择左上角选择电源按钮的功能

	![battle1](/assets/2018/09/11/battle1.jpg)

	 选择更改当前不可用的设置

	![battle2](/assets/2018/09/11/battle2.jpg)

	关闭快速启动

	![battle3](/assets/2018/09/11/battle3.jpg)

保存修改

5. 重启 选择从 U 盘启动 具体方法百度搜索 "你的电脑品牌+进入bios方法"

##安装过程

0. 进入 `Ubuntu` 安装界面后选择 "Try ubuntu without install" 或者 "Install ubuntu"

	![begin](/assets/2018/09/11/begin.png)

1. 将进度条拉到下 可见简体中文

	![Chinese](/assets/2018/09/11/Chinese.png)

2. 键盘设置 如果在第一步已经设置了语言中文 则默认汉语键盘

	![Keyboard](/assets/2018/09/11/Keyboard.png)

3. 选择正常安装

	![Install](/assets/2018/09/11/Install.png)

4. 这里不要选择清楚磁盘案子 `Ubuntu` 而是应该使用 "安装 Ubuntu 与 Windows boot manager 共存"

![Start](/assets/2018/09/11/Start.png)

5. 弹出分区发生改变 请点击是 然后 接着设置时区 用户名 即可。等待一段时间，选择立即重启，拔出 U 盘

##安装之后的设置

```bash

#卸载原有输入法并使用 Fcitx 输入法及 Sunpinyin 

sudo apt remove ibus

sudo apt install fcitx fcitx-sunpinyin fcitx-pinyin -y

#在设置里更改为更适合自己的设置 从右上角托盘或者活动进入设置

#安装 C 编译器 VIM 编辑器等软件

sudo apt install vim gcc git -y
```

![set](/assets/2018/09/11/set.png)

现在享受它吧。
