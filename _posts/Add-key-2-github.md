---

layout: post

title:  "如何向 Github 添加 ssh key"

date:   2018-05-21 19:34:01 +0800

categories: tutorial

---



0.  如果你的电脑还没有安装 `SSH` , 先使用包管理器安装

1.  生成 `SSH` 密钥对 

   ```
   ubuntu@ubuntu1:~$ ssh-keygen
   Generating public/private rsa key pair.
   Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
   Enter passphrase (empty for no passphrase): 
   Enter same passphrase again: 
   Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
   Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
   The key fingerprint is:
   SHA256:EfVw8ti8MM7HCPqQZSsdNjR8zwqZ1vMHSuJ1oNEtPlA ubuntu@ubuntu1
   The key's randomart image is:
   +---[RSA 2048]----+
   |       .++E..    |
   |       .+o=X.    |
   |        OX+==    |
   |       BBOB==.   |
   |      =oS=+B+.   |
   |       +. o.. .  |
   |        .    .   |
   |                 |
   |                 |
   +----[SHA256]-----+
   ​```
   ```

   

2.  查看公钥内容

   ```
   ubuntu@ubuntu1:~$ cat .ssh/id_rsa.pub 
   ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCv/t7Qj0BbnwCGn2PQg1qYNYxEO2UQyN/2aD8ujr0y0vHHF/0RXp2d4gnZkNTUOZfhnrhTVceT7hi7a+JBXWq7Ceatpy5JW30wmpw2RDtR5yIEDiOLoIFXVtPRSo4BpmDCQGvGw4ihRRuUvltq3LinaWqAPtTStq2xiPyC/ZRNMdHRB9zFMnoDDsZzaQNKvXumpnAmK9KzdFsEdmUpQ3oOMwXAMgMkUVZYscf9LbLCfoDDJK4ThYPb6lSMrFlmXonKJtwdccw2/gMVqWaK1c3fsoOr1nTEjq4tuCOwXNFXEg4fUJrx5ws3zbr0OJ4DCrZSoBNAy9v9WfT+vAA4EZ2V ubuntu@ubuntu1
   ​```
   ```

3. 复制内容到 `Github`

    

   点击右上角自己的头像后

   ![hp](/assets/2018/05/21/hp.png)

   点击设置选择 `SSH and GPG keys` 

   ![ssh](/assets/2018/05/21/ssh.png)

   点击 `New SSH key` 并复制文件内容至此

   ![newkey](/assets/2018/05/21/newkey.png)