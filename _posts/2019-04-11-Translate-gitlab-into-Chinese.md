---
layout: post
title: "不知道怎么就搞成了代理安装"
author: licheng
date: 2019-04-11 14:23 +800
category: experiance
---

*Gitlab 已经安装完成，本来汉化内容过于简单没有必要另起一篇的。但因为 Gitlab的服务器在国外。使用 git 获取 gitlab 的时候接收对象永远停在了百分之八十八，所以不得不考虑安装代理另谋出路*

## 安装代理

1. 首先删除掉克隆接近完成却一直没有然后的仓库

```
[shiptux@localhost ~]$ git clone https://gitlab.com/xhang/gitlab.git
正克隆到 'gitlab'...
remote: Enumerating objects: 951929, done.
remote: Counting objects: 100% (951929/951929), done.
remote: Compressing objects: 100% (195668/195668), done.
... 对象中:  88% (838194/951929), 168.57 MiB | 168.00 KiB/s   
^C
```

2. 安装科学上网软件

`Centos` 本身默认的软件源比较保守，缺乏很多软件。所以需要安装一下 `epel` 软件源代作为扩展。 `epel` 的全称是 `Extra Packages for Enterprise Linux`。旨在为 `Linux` 提供高质量的软件项目。 

首先安装 `epel` 源

<code>sudo yum install epel-release -y</code>

然后安装 `pip` 

<code>sudo yum install python-pip -y</code>

更新 `pip` 本体

<code>sudo pip install --upgrade pip</code>

安装某个软件

<code>sudo pip install 某软件名</code>

编辑一下 `Json` 格式的配置文件

<code>vim /etc/sslocal.json</code>
```bash
{
    "server":"your_server_ip", #ss服务器IP

    "server_port":your_server_port, #端口

    "local_address":"127.0.0.1", #本地ip

    "local_port":1080, #本地端口

    "password":"your_server_passwd",#连接ss密码

    "timeout":300, #等待超时

    "method":"rc4-md5", #加密方式 这里要和服务器的一致。

    "fast_open":false, # true 或 false
}
```

根据自己的情况而填写。完成后可以使用 

<code>sudo sslocal -c /etc/ssloal.json</code>

运行。可以使用 `nohup` 等程序来让它后台运行。

这里使用 `systemctl` 来控制会更方便

使用 `VIM` 编辑器 

<code>sudo vim /etc/systemd/system/sslocal.service</code>

写入以下内容

```bash
[Unit]
Description=SS server
After=syslog.target network.target
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/bin/sslocal -c /etc/sslocal.json
ExecStop=/usr/bin/sslocal -d stop
RestartSec=1min
KillMode=control-group
Restart=always

[Install]
WantBy=Multi-user.target
```

<code>sudo systemctl enable sslocal.service</code> 开机自启

<code>sudo systemctl start sslocal.service</code> 启动服务

<code>sudo systemctl status sslocal.service</code> 查看该服务状态

因为安装的某个软件提供的代理是 `sock` 代理，本身很多服务并不支持。所以安装 `provixy` 来实现强制使用。

3. 安装 `proxychains` 实现强制性 `socks` 代理

下载源代码

<code>git clone git@github.com:rofl0r/proxychains-ng.git<code>

*因为在服务器上 密钥鉴定失败 于是用该服务器生成了一个密钥对上传到 github 上*

```bash
[shiptux@localhost ~]$ git clone git@github.com:rofl0r/proxychains-ng.git
正克隆到 'proxychains-ng'...
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
[shiptux@localhost ~]$ ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/shiptux/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/shiptux/.ssh/id_rsa.
Your public key has been saved in /home/shiptux/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:QMpjn9iuwegO+4a4hdU72erpKLKG1n7jfaHOOhHvyeQ shiptux@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
|      .          |
|   . o           |
|    = .          |
|   o * o         |
|  . o * S        |
| o o * o .       |
|=.+ * O o .      |
|*B.o XoE .       |
|BB*+Oo=+.        |
+----[SHA256]-----+
[shiptux@localhost ~]$ 
[shiptux@localhost ~]$ cat ~/.ssh/id_rsa
id_rsa      id_rsa.pub  
[shiptux@localhost ~]$ cat ~/.ssh/id_rsa
id_rsa      id_rsa.pub  
[shiptux@localhost ~]$ cat ~/.ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAZor7l/b1ZLJXBsm2LyOYXKRBXpbCkslMFjVs/VGjrZgMvPQZv/6pYdNZbVDLFeT4srX2nA35NHuupI6QJsqMAcmHc+JjtNkGG0yOa2Vj+QpplIjK+09NqAqOIP3hQUcOrmM4qIddG3FS8vt5f8D3h4ryIlmmx1zQDe7YIhTG9dbM+aACJupcT0AiZpvBGhT9wS/jHe8iW3noiULj4WK7YCaxnDLTpKjyO5vYJNtzoDUcEVINJS1FKJqv4unmN2VTWno3k/Kasrlvi62ydq4+hUHUizeKdqifAkC6NP7IFqqYggF3oU1bPh1aihmpk/MrIzL/ax8F3ZkCco2cCW4N shiptux@localhost.localdomain
```

克隆完成后进入该目录进行编译和安装工作

```
make: cc：命令未找到
make: *** [src/nameinfo.o] 错误 127
## 错误提示 C 编译器未发现 先要安装编译器
```

<code>sudo yum install gcc -y</code>

```
make ##重新执行编译

```bash
cd proxychains-ng
./configure --prefix=/usr --sysconfdir=/etc 
make
make install
cd .. && rom -rf proxychains-ng
```

proxychain 的配置文件

```bash
dynamic_chain
chain_len = 1
proxy_dns
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000
[ProxyList]
socks5 127.0.0.1 1080
```

接下来使用命令的时候可以使用

<code>proxychains4 git clone https://gitlab.com/xhang/gitlab.git</code>

使用代理拉取，虽然还是很慢 )： 

参考资料:
[proxychain](https://github.com/rofl0r/proxychains-ng)
[利用proxychains在终端使用代理](https://blog.csdn.net/gengxuelei/article/details/52514603)
[linux配置某软件](https://www.cnblogs.com/jehorn/p/10161074.html)
[CentOS下安装shadowsocks代理的方法](https://www.jianshu.com/p/11585058cbe9)
[什么是epel](https://www.cnblogs.com/gaoyuechen/p/7683471.html)
[Gitlab汉化指南](https://gitlab.com/xhang/gitlab/wikis/home)
