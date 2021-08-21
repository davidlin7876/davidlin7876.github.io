---
title: zsh: command not found pip 解决方法
date: 
tags: [Mac]
categories: 学习笔记
description: 
top: 
---
# 出现zsh: command not found: xxx解决方法：

> 把 bash shell 中.bash_profile 全部环境变量加入zsh shell里就好

### step1:

Term执行
```
open .zshrc
```

### step2:

找到 “# User configuration”
加入
```
source ~/.bash_profile
```

![](https://img2018.cnblogs.com/blog/1346973/201901/1346973-20190122105953281-1247190119.png)


### step3:

Term执行
```
source .zshrc
```
