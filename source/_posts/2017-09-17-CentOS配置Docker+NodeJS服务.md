---
title: CentOS配置Docker+NodeJS服务
date: 
tags: [NodeJS]
categories: 
description: 
top: 
---
Mac端的本地环境请点击：[Mac配置Docker+NodeJS服务](http://www.jianshu.com/p/e33815e71e35)
# 安装
安装 NodeJS
[https://nodejs.org/en/download/](https://nodejs.org/en/download/ "https://nodejs.org/en/download/")
准备命令：
```
yum -y install gcc make gcc-c++ openssl-devel wget
```
下载源码及解压：
```
wget https://nodejs.org/dist/v6.11.3/node-v6.11.3.tar.gz
tar -zvxf node-v6.11.3.tar.gz
```
检查所需要配置
```
./configure
```
编译及安装：
```
make && make install
```
验证是否安装配置成功：
```
node -v
```
------------
安装loopback-cli
[http://loopback.io](http://loopback.io "http://loopback.io")
```
npm install -g loopback-cli
```
安装git
```
yum -y install git
```
安装cnpm
[https://npm.taobao.org/](https://npm.taobao.org/ "https://npm.taobao.org/")
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
安装zsh作为默认SHELL
[http://blog.csdn.net/w670328683/article/details/49782601](http://blog.csdn.net/w670328683/article/details/49782601 "http://blog.csdn.net/w670328683/article/details/49782601")
```
yum -y install zsh
chsh -s /bin/zsh
echo $SHELL
```
安装oh-my-zsh美化zsh
[https://github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh "https://github.com/robbyrussell/oh-my-zsh")
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```
安装docker和docker-compose
[http://www.linuxidc.com/Linux/2014-12/110034.htm](http://www.linuxidc.com/Linux/2014-12/110034.htm "http://www.linuxidc.com/Linux/2014-12/110034.htm")
```
yum install docker
yum install docker-compose
```
安装gem
```
yum install -y gem
```
安装mosh
```
yum install mosh
```
安装Vim的插件janus
The distribution also requires ack, ctags, git, ruby and rake. 
[https://github.com/carlhuda/janus](https://github.com/carlhuda/janus "https://github.com/carlhuda/janus")
```
curl -L https://bit.ly/janus-bootstrap | bash
```
安装httpie
```
yum install httpie
```
------------
# centOS的一些命令：
docker的一些命令
```
docker-compose down
docker-compose up -d
service docker start
chkconfig docker on
```
安装caddy-docker例子,测试docker功能
[https://github.com/abiosoft/caddy-docker](https://github.com/abiosoft/caddy-docker "https://github.com/abiosoft/caddy-docker")
```
docker run -d -p 2015:2015 abiosoft/caddy
```
查看log
```
docker ps
docker exec -it xiaoyx_ws_1 sh
pm2 list
pm2 logs 1
```
硬盘
```
df -h
```
内存
```
free -h
```
目录详情
```
ls -lh
```
安装路径。
```
which node
```
连接服务器（免密码）
```
ls .ssh
ssh-keygen
ssh-copy-id root@dlin.top
ssh root@dlin.top
```
mosh连接服务器
```
mosh root@dlin.top
```
```
curl http://0.0.0.0:2015
curl http://google.com
```
```
ip addr
```