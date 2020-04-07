---
layout: post
title: "把Hexo部署到vps"
date: 2020-04-06 16:26
comments: true
tags: 
  - hexo
  - blog
---

#### 准备工作

1、假设本地已经安装好hexo环境

1.1 购买服务器.

1.2 购买域名,可以从阿里云购买,之后在控制台进行域名解析即可。

2、购买vps以后，可通过`ssh`登录服务器操作。

`ssh root@155.138.x.x`

![](https://s1.ax1x.com/2020/04/05/GrdtbD.png)

<!--more-->

##### 安装相关软件git

然后安装git：`git --version` // 如无，则安装

`yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel && yum install -y git`

1. 创建用户并配置其仓库，执行如下命令

   `useradd git`

   `passwd git` // 设置密码

   `su git `

  ` cd /home/git/`

  ` mkdir -p projects/blog ` // 存放编译好的web文件，供nginx访问

   `mkdir repos && cd repos`

   `git init --bare blog.git` // 创建一个裸露的仓库

  ` cd blog.git/hooks`

  ` vi post-receive `// 创建hook钩子（后面单独说，很有用的一个功能），输入

   `#!/bin/sh`
   `git --work-tree=/home/git/projects/blog --git-dir=/home/git/repos/blog.git checkout -f`

   添加完毕后修改权限，执行如下命令

   `chmod +x post-receive`  //给钩子添加执行权限

   `exit `// 退出到 root 

   `chown -R git:git /home/git/repos/blog.git` // 添加权限

   测试git是否能用：在本地找空白文件夹，执行如下命令

   `git clone [git@server_ip:/home/git/repos/blog.git](mailto:git@server_ip:/home/git/repos/blog.git)`

   建立ssh信任关系，在本地电脑，执行如下命令

   `ssh-copy-id -i ~/.ssh/id_rsa.pub git@server_ip`

   `ssh git@server_ip` // 测试能否登录
   

   ------

   

2. 安装nginx

   下载并安装`nginx`，执行如下命令

   `cd /usr/local/src`

   `wget http://nginx.org/download/nginx-1.15.2.tar.gz`

   `tar xzvf nginx-1.15.2.tar.gz`

   `cd nginx-1.15.2`

   `./configure `// 如果后面还想要配置 SSL 协议，就执行后面一句！

   `./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-file-aio --with-http_realip_module`

   `make && make install`

   `alias nginx='/usr/local/nginx/sbin/nginx'` // 为nginx取别名，后面可直接

   `yum -y install gcc gcc-c++ autoconf automake make`

   先启动是否安装成功，执行如下命令 ` nginx`

   `nginx -s stop` // 先停止nginx*

   `cd /usr/local/nginx/conf`

   `vi nginx.conf`

   修改root 解析路径，如下图

   ![](https://s1.ax1x.com/2020/04/06/GyFi1P.png)

   *同时将* *user 改为 root 如下图，如下图*

   ![](https://s1.ax1x.com/2020/04/06/GyibSx.png)

   `nginx -s reload`

   

3. 修改本地hexo的配置文件`_config.yml`文件

   编辑_config.yml的`deploy`

   ![](https://s1.ax1x.com/2020/04/06/GyiUQf.png)

4. 在本地hexo目录下执行 `hexo clean  &&  hexo g &&  hexo d`



### 错误解决

1. `nginx: [error] open() "/usr/local/nginx/logs/nginx.pid" failed (2: No such file or directory)`

   解决：使用指定nginx.conf文件的方式重启：`nginx/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf`

2. `nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)`

   解决：`ps -aux | grep nginx  &&  kill -9 pid`

   