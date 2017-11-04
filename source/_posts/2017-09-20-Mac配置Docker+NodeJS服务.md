---
title: Mac配置Docker+NodeJS服务
date: 
tags: [NodeJS]
categories: 学习笔记
description: 
top: 
---
服务器端环境配置请点击：[CentOS配置Docker+NodeJS服务](http://www.jianshu.com/p/12cee6725b58)

# Mac的一些安装

### 安装 NodeJS 
[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

### 安装loopback-cli
[http://loopback.io](http://loopback.io "http://loopback.io")
```
npm install -g loopback-cli
```

安装zsh作为默认SHELL
[http://blog.csdn.net/w670328683/article/details/49782601](http://blog.csdn.net/w670328683/article/details/49782601 "http://blog.csdn.net/w670328683/article/details/49782601")
```
brew install zsh
zsh --version
chsh -s /bin/zsh
echo $SHELL
```
安装oh-my-zsh美化zsh
[https://github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh "https://github.com/robbyrussell/oh-my-zsh")
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

### 安装tmux分屏幕
[https://github.com/gpakosz/.tmux](https://github.com/gpakosz/.tmux "https://github.com/gpakosz/.tmux")
```
brew install tmux
```
### 安装mosh
```
brew install mosh
```

### 安装mongodb数据库(只在docker使用的话不用装)
```
brew update
brew install mongodb
```
##### 遇到一个问题

```
Error: Running Homebrew as root is extremely dangerous and no longer supported.
As Homebrew does not drop privileges on installation you would be giving all
build scripts full access to your system.
```
##### 这样解决
```
sudo chown -R $(whoami) /usr/local
brew install mongodb
```

### 安装redis数据(只在docker使用的话不用装)

[https://redis.io](https://redis.io)

```
tar xzf redis-4.0.1.tar
cd redis-4.0.1
make
make install
```
### 安装mongo-express(只在docker使用的话不用装)
[https://github.com/mongo-express/mongo-express](https://github.com/mongo-express/mongo-express)

```
npm install -g mongo-express
```

### 安装Vim的插件janus
The distribution also requires ack, ctags, git, ruby and rake. 
[https://github.com/carlhuda/janus](https://github.com/carlhuda/janus "https://github.com/carlhuda/janus")
```
curl -L https://bit.ly/janus-bootstrap | bash
```
### 安装pm2
http://pm2.keymetrics.io
```
npm install pm2 -g
npm install pm2@latest -g
```
### 安装docker，docker-compose
[https://www.docker.com/](https://www.docker.com/)
```
brew search docker
brew update
brew install docker
brew install docker-compose
```
### 安装httpie
```
brew install httpie
```
# Mac的代码管理：
```
git clone https://git.coding.net/zbsas/webstore-backend.git
git checkout next
git submodule init
git submodule update
npm install
```
发现子模块代码下不来，这样解决：
查看公钥私钥
```
cd ~/.ssh
```
.pub 文件是你的公钥，另一个则是私钥。如果找不到这样的文件，你可以通过运行 程序来创建它们：

```
ssh-keygen
```

然后在coding上设上公钥，子目录代码可以下来了

```
git submodule update
```

### 上传服务器代码
```
rsync -avuzb xiaoyx root@dlin.top:~/
```
##### 过滤没有必要的文件和目录
```
rsync -avuzb --exclude=.git --exclude=node_modules webstore-backend/* root@dlin.top:~/
```

### 下载服务器代码
```
rsync -avuzb root@dlin.top:/root/xiaoyx/ .
scp root@dlin.top:/root/xiaoyx/docker-compose.yml .
```

# 代码调试：

### 开启redis

```
src/redis-server
```
也可以用pm2开启redis
```
pm2 start src/redis-server
```

### 开启mongodb
```
cd /Code-Backend/mongodb
mkdir data
mkdir log
mongod --dbpath data --logpath log/mongod.log --logappend --fork
```
### 关闭mongodb
```
mongo
use admin
db.shutdownServer();
```
### pm2 看log
```
cd /Code-Backend/webstore-backend
pm2 start development.json
pm2 logs 1
pm2 stop all
```
---
# docker调试
[Docker之Compose服务编排](http://www.cnblogs.com/52fhy/p/5991344.html)

##### 开启和关闭
```
docker ps
docker-compose up -d
docker-compose stop
docker-compose restart
docker-compose down
```
##### 重置一个docker
```
docker-compose down
docker ps -a
docker rm codebackend_ws_1
docker ps -a
docker image ls
docker image rm codebackend_ws
docker-compose up -d
```
##### 启动并看log
```
docker-compose up
```
##### 看其中某一个log
```
docker ps
docker exec -it codebackend_ws_1 sh
pm2 list
pm2 logs 1
```
##### 批处理脚本

```
# 关闭所有正在运行容器
docker ps | awk  '{print $1}' | xargs docker stop
# 删除所有容器应用
docker ps -a | awk  '{print $1}' | xargs docker rm
# 或者
docker rm $(docker ps -a -q)
```


### mongo-express
[https://github.com/mongo-express/mongo-express](https://github.com/mongo-express/mongo-express)

#### 先建立用户
```
docker exec -it codebackend_mongodb_1 mongo
show dbs;
use admin
```
##### 管理员用户
```
 db.createUser({
    user : "用户名",
    pwd  : "密码",
    roles : [
        {
            role : "userAdminAnyDatabase",
            db : "admin"
        }
    ]
 })
```

##### 特定数据库管理权限的用户
```
use "数据库名"
 db.createUser({
    user : "用户名",
    pwd  : "密码",
    roles : [
        {
            role : "userAdmin",
            db : "数据库名"
        }
    ]
 })
```

##### 一般用户
```
db "数据库名"
 db.createUser({
    user : "用户名",
    pwd  : "密码",
    roles: [
        {
            role : "read",  # or "readWrite"
            db : "数据库名",
        }
    ]
 })
```


> 例：创建一个数据库用户，对该数据具有读写权限
> 
> 创建一个对数据库具有读写权限的数据库用户 
> use dbname ; 
> db.createUser({user: “dbuser”, pwd: “dbuseradmin”, roles:[{role: “readWrite”, db: “dbname”}] })
> 
> 数据库用户登录 
> mongo dbname -u dbuser -p dbduseradmin


```
cd YOUR_PATH/node_modules/mongo-express/ && node app.js
mongo-express -u user -p password -d database
mongo-express -u dlin -p dlin -d webstore-prod
```
##### 远程
```
mongo-express -u user -p password -d database -H mongoDBHost -P mongoDBPort
```
##### 其他
```
mongo-express -a -u superuser -p password
mongo-express -h
```

##### Usage (Docker)
```
docker build -t mongo-express .
docker run -it --rm -p 8081:8081 --link YOUR_MONGODB_CONTAINER:mongo mongo-express
```
例子：
```
docker run -it --rm -p 8081:8081 --link codebackend_mongodb_1:mongo mongo-express
```

You can use the following environment variables to modify the container's configuration:

Name                              | Default         | Description
----------------------------------|-----------------|------------
`ME_CONFIG_MONGODB_SERVER`        |`mongo` or `localhost`| MongoDB host name or IP address. The default is `localhost` in the config file and `mongo` in the docker image. If it is a replica set, use a comma delimited list of the host names.
`ME_CONFIG_MONGODB_PORT`          | `27017`         | MongoDB port.
`ME_CONFIG_MONGODB_URL`           | `mongodb://admin:pass@localhost:27017/db?ssl=false`
`ME_CONFIG_MONGODB_ENABLE_ADMIN`  | `false`         | Enable administrator access. Send strings: `"true"` or `"false"`.
`ME_CONFIG_MONGODB_ADMINUSERNAME` | ` `             | Administrator username.
`ME_CONFIG_MONGODB_ADMINPASSWORD` | ` `             | Administrator password.
`ME_CONFIG_MONGODB_AUTH_DATABASE` | `db`            | Database name (only needed if `ENABLE_ADMIN` is `"false"`).
`ME_CONFIG_MONGODB_AUTH_USERNAME` | `admin`         | Database username (only needed if `ENABLE_ADMIN` is `"false"`).
`ME_CONFIG_MONGODB_AUTH_PASSWORD` | `pass`          | Database password (only needed if `ENABLE_ADMIN` is `"false"`).
`ME_CONFIG_SITE_BASEURL`          | `/`             | Set the express baseUrl to ease mounting at a subdirectory. Remember to include a leading and trailing slash.
`ME_CONFIG_SITE_COOKIESECRET`     | `cookiesecret`  | String used by [cookie-parser middleware](https://www.npmjs.com/package/cookie-parser) to sign cookies.
`ME_CONFIG_SITE_SESSIONSECRET`    | `sessionsecret` | String used to sign the session ID cookie by [express-session middleware](https://www.npmjs.com/package/express-session).
`ME_CONFIG_BASICAUTH_USERNAME`    | `admin`         | mongo-express web login name. Sending an empty string will disable basic authentication.
`ME_CONFIG_BASICAUTH_PASSWORD`    | `pass`          | mongo-express web login password.
`ME_CONFIG_REQUEST_SIZE`          | `100kb`         | Used to configure maximum mongo update payload size. CRUD operations above this size will fail due to restrictions in [body-parser](https://www.npmjs.com/package/body-parser).
`ME_CONFIG_OPTIONS_EDITORTHEME`   | `rubyblue`      | Web editor color theme, [more here](http://codemirror.net/demo/theme.html).
`ME_CONFIG_SITE_SSL_ENABLED`      | `false`         | Enable SSL.
`ME_CONFIG_MONGODB_SSLVALIDATE`   | `true`          | Validate mongod server certificate against CA
`ME_CONFIG_SITE_SSL_CRT_PATH`     | ` `             | SSL certificate file.
`ME_CONFIG_SITE_SSL_KEY_PATH`     | ` `             | SSL key file.
`ME_CONFIG_SITE_GRIDFS_ENABLED`   | `false`         | Enable gridFS to manage uploaded files.


Example:

```
docker run -it --rm \
    --name mongo-express \
    --link web_db_1:mongo \
    -p 8081:8081 \
    -e ME_CONFIG_OPTIONS_EDITORTHEME="ambiance" \
    -e ME_CONFIG_BASICAUTH_USERNAME="" \
    mongo-express
```
##### 设置dockor运行的配置文件
**docker-compose.yml**

```
mongo-express:
    image: mongo-express
    depends_on:
      - mongodb
    restart: always
    links:
      - mongodb:mongo
    ports:
      - "8081:8081"
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME = admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD = pass
```

##### 设置登录页账号密码

[https://caddyserver.com/docs/basicauth](https://caddyserver.com/docs/basicauth)
##### Caddyfile

```
http://db.local.dlin.top {
  basicauth / admin pass
  proxy / mongo-express:8081 {
    transparent
  }
}
```