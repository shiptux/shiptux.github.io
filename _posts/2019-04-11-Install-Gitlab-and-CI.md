---
layout: post
title: "在 Centos 下安装 Gitlab "
date: 2019-04-11 10:30 +800
author: licheng
category: experiance
---

*Gitlab 是一个开源的代码托管平台，类似于 Github 。同时它也提供了供个人，企业以及组织搭建自己的代码托管平台的功能。Gitlab 提供两个分支, ce 和 ee分别是企业版和社区版。一般情况下使用社区版本就已经足够。*

## 在 Centos 7 下安装 Gitlab

根据[官方文档](https://about.gitlab.com/install/#centos-7)来安装非常的简单，不需要手动修改软件源设置，也不需要其它复杂的设置。根据自己的需要可以选用安装 `rpm` 包或者是 直接使用 `Docker` 来进行安装。

    这里使用 `RPM` 方式安装 `Gitlab` 平台。

    1. 首先安装所需的依赖软件

    ```
    sudo yum install -y curl policycoreutils-python openssh-server #安装几个必要的基础软件
    sudo systemctl enable sshd #允许 sshd 服务开机自启
    sudo systemctl start sshd #启动 sshd 服务
    sudo firewall-cmd --permanent --add-service=http #防火墙允许 http 协议
    sudo systemctl reload firewalld #重载防火墙
    ```

    2.接下来安装 `Postfix` 。一个邮件服务器，为了用来发送提醒邮件。

    ```
    sudo yum install postfix #安装
    sudo systemctl enable postfix #开机自启动
    sudo systemctl start postfix #启动
    ```  

    3. 之前安装的时候往往需要手动修改软件源。*比如百度搜索出来的大多数都是这样的* 但是我尝试安装后发现官方的脚本已经可以自动修改安装源了。自动使用的是 `TUNA` 软件源。

    <code>curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash</code>

    4. 接下来使用 `yum` 安装 `Gitlab` 即可。

    <code>sudo yum install -y gitlab-ce</code>

## Gitlab 的相关配置 

    1. 配置 `Gitlab` 的外部连接

    修改 `Gitlab` 的配置文件。该文件在`/etc/gitlab/gitlab.rb`。修改这行 <code>external_url "http://gitlab.example.com" </code>。 因为安装在这里的 `Gitlab` 并没有打算对外服务。如果读者刚好跟我一样是某校的，提供一个某校某老师搭建在内网的 `Gitlab` [地址](10.1.1.50)给你尝试。

    2. 修改好配置文件之后执行

    ```
    sudo gitlab-ctl reconfigure #Tips:Gitlab的命令行控制是 gitlab-ctl
    sudo gitlab-ctl start
    ```

    3. 在浏览器中输入配置的地址即可登陆。初次登录设置 root 账户的密码。 之后就可以看到相关的配图。  

    