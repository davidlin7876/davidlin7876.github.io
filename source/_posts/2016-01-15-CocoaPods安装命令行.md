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