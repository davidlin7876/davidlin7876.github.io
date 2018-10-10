---
title: CocoaPods安装命令行
date: 
tags: [iOS]
categories: 
description: 
top: 
---
1. 安装xcode工具
```
xcode-select --install
```


2. 安装rvm
```
curl -L get.rvm.io | bash -s stable

rvm list known （找到最新的rvm版本）

rvm install 2.4
```


3.改数据源 
```
gem sources -a https://gems.ruby-china.org

gem sources --remove https://rubygems.org/
```


5. 安装rails
```
sudo gem install rails
```


6. 安装cocoapods
```
sudo gem install cocoapods
```

在cocoapods 执行 sudo gem install cocoapods 的时候出现  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /usr/bin directory.
改为 sudo gem install -n /usr/local/bin cocoapods  即可