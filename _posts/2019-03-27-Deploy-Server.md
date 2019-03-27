---
layout: post
title: "智慧农业监测系统的云服务器完整部署过程"
date: 2019-03-27 10:49 +800
categorys:  教程
---

# 这篇文章描述智慧农业监测系统的云服务器完整部署过程

## 树莓派

1. 连接到树莓派

    <code>ssh pi@192.168.43.xx</code>

    如果忘记地址 可以在 `known_hosts` 里面查看

    <code>cat ~/.ssh/known_host</code>

2. 查看树莓派上的代码

    使用命令行编辑查看代码

    <code>nano scpmydht11.c</code>

    使用命令行编译下位机代码

    <code>gcc scpmydht11.c -o samsclient -l wringPi</code>

3. 运行树莓派上的代码

    确保服务器上的程序先启动  *Tips:在Linux下Ctrl+c可以停止程序*

    <code>./samsclient</code>

## 服务器

1. 连接到服务器 

    在桌面上右键选择 `git bash here` 在弹出的 `git bash` 命令行中

    <code>ssh liujinxiu@shiptux.cn</code> 并输入密码 *Tips:Linux 带有密码位数保护 所以**不会显示*

    在成功连接服务器之后 左边的用户@host会发生变化。

2. 编译服务器代码

    成功连接后可以在用户的目录下找到服务器的代码和执行文件. **切记运行先服务程序再运行客户端程序，关闭的顺序同样。可以使用 Ctrl+c关闭**

    ```bash
        ls  ## 列出当前目录下所有文件
        server.c server
        gcc server.c -o server /usr/lib64/mysql/libmysqlclient.so.18 ##编译命令 gcc 调用编译器 server.c 是源代码文件 -o 自动链接 server 是生成的执行文件名 /usr/lib64....等是这个代码的头文件中需要的动态链接库
        sudo ./server ##使用 root 即超级用户权限执行该文件 
        同样可以使用 Ctrl + c关闭程序   
    ```

3. 代码文件的查看

    `Vim` 版本

    <code> vim server.c</code> *Tips:注意后缀不要少了，否则会变成用编辑器打开二进制文件，那会是一团乱码*

    使用方向键浏览 几个常用的命令 按 `i` 键进入插入模式 `esc` 键退出该模式 `:w`写入 具体是确保不在编辑模式 然后先按 ":" 冒号 再输入 "w" 同样 `:wq` 是写入并退出 `:q`是推出 如过加上 !那就是强制执行

    `Nano` 版本

    `Nano` 版本要比 `Vim` 简单许多

    <code>nano server.c</code>

    同样是方向键浏览，常用命令已经写在了底部。

4. Web 程序 `Tomcat` 的启动和重启

    <code>sudo /opt/tomcat/bin/startup.sh</code>  启动

    <code>sudo /opt/tomcat/bin/shutdown.sh</code> 关闭

