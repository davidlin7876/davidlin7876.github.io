---
title: Flutter SDK安装和诊断
date: 
tags: [iOS,Android]
categories: 学习笔记
description: 
top: 
---
[TOC]
# 安装
本文以Mac系统为例

## 访问官网安装：
https://flutter.dev/docs/get-started/install/macos

### 下载SDK包flutter_macos_v1.2.1-stable.zip

### 命令行执行下面几个步骤：
```
➜ cd ~/development //替换成自己的目录
➜ unzip ~/Downloads/flutter_macos_v1.2.1-stable.zip
➜ export PATH="$PATH:`pwd`/flutter/bin"
➜ flutter precache
➜ flutter doctor
```
<!--more-->
### doctor提示问题
```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.2.1, on Mac OS X 10.14.3 18D109, locale
    zh-Hans-CN)
[!] Android toolchain - develop for Android devices (Android SDK version 26.0.1)
    ✗ Flutter requires Android SDK 28 and the Android BuildTools 28.0.3
      To update using sdkmanager, run:
        /Users/david/Library/Android/sdk/tools/bin/sdkmanager
        "platforms;android-28" "build-tools;28.0.3"
      or visit https://flutter.io/setup/#android-setup for detailed
      instructions.
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor
      --android-licenses
[!] iOS toolchain - develop for iOS devices (Xcode 10.2)
    ✗ libimobiledevice and ideviceinstaller are not installed. To install with
      Brew, run:
        brew update
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install:
        brew install ios-deploy
[!] Android Studio (version 2.3)
    ✗ Flutter plugin not installed; this adds Flutter specific functionality.
    ✗ Dart plugin not installed; this adds Dart specific functionality.
[!] VS Code (version 1.7.2)
    ✗ Flutter extension not installed; install from
      https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter
[!] Connected device
    ! No devices available

! Doctor found issues in 5 categories.
```

# 解决中遇到的问题
## 1，运行precache很慢
```
Downloading android-arm64-profile/darwin-x64 tools... ⢿ (This is taking an unexpect
```
### 解决方案
```
➜ export PUB_HOSTED_URL=https://pub.flutter-io.cn
➜ export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
➜ flutter precache
```

## 2，更新brew很慢
### 解决方案
使用中科大的镜像替换默认源
- 第一步，替换brew.git
```
➜ cd "$(brew --repo)"
➜ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```
- 第二步：替换homebrew-core.git
```
➜ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
➜ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```
- 最后使用下面命令进行更新，发现速度变的很快。替换镜像完成。
```
➜ brew update
```
## 3，brew遇到SSL certificate problem
```
➜ brew install --HEAD usbmuxd
==> Cloning https://git.sukimashita.com/libusbmuxd.git
Cloning into '/Users/david/Library/Caches/Homebrew/usbmuxd--git'...
fatal: unable to access 'https://git.sukimashita.com/libusbmuxd.git/': SSL certificate problem: certificate has expired
Error: An exception occurred within a child process:
  DownloadError: Failed to download resource "usbmuxd"
Failure while executing; `git clone --branch master https://git.sukimashita.com/libusbmuxd.git /Users/david/Library/Caches/Homebrew/usbmuxd--git` exited with 128. Here's the output:
Cloning into '/Users/david/Library/Caches/Homebrew/usbmuxd--git'...
fatal: unable to access 'https://git.sukimashita.com/libusbmuxd.git/': SSL certificate problem: certificate has expired
```
### 解决方案
```
➜ git config --global http.sslVerify false
```

## 4，android-licenses长时间没有反应

```
➜ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v1.3.8, on Mac OS X 10.14.3 18D109, locale zh-Hans-CN)
⢿ Checking Android licenses is taking an unexpectedly long time...
```
### 解决方案
```
➜ flutter doctor --android-licenses
Warning: File /Users/david/.android/repositories.cfg could not be loaded.
All SDK package licenses accepted.======] 100% Computing updates...
```

## 5，Install the Flutter and Dart plugins
### Android Studio 解决方案
To install these:
- Start Android Studio.
- Open plugin preferences (Preferences > Plugins on macOS, File > Settings > Plugins on Windows & Linux).
- Select Browse repositories, select the Flutter plugin and click Install.
- Click Yes when prompted to install the Dart plugin.
- Click Restart when prompted.

### VSCode 解决方案
- Start VS Code.
- Invoke View > Command Palette….
- Type “install”, and select Extensions: Install Extensions.
- Type “flutter” in the extensions search field, select Flutter in the list, and click Install. This also installs the required Dart plugin.

#### VSCode Validate your setup with the Flutter Doctor
- Invoke View > Command Palette….
- Type “doctor”, and select the Flutter: Run Flutter Doctor.
- Review the output in the OUTPUT pane for any issues.


## 6，Android SDK版本不匹配
```
➜ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v1.3.8, on Mac OS X 10.14.3 18D109, locale
    zh-Hans-CN)
[!] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
    ✗ Flutter requires Android SDK 28 and the Android BuildTools 28.0.3
      To update using sdkmanager, run:
        /Users/david/Library/Android/sdk/tools/bin/sdkmanager
        "platforms;android-28" "build-tools;28.0.3"
      or visit https://flutter.io/setup/#android-setup for detailed
      instructions.
[✓] iOS toolchain - develop for iOS devices (Xcode 10.2)
[✓] Android Studio (version 3.3)
[✓] VS Code (version 1.32.3)
[✓] Connected device (1 available)
```
### 解决方案
- 点击androidStudio菜单的Settings
- 点击Appearance&Behavior
- 点击System Settings
- 安装Android SDK 28


## 最后所有诊断都通过
```
➜ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel beta, v1.3.8, on Mac OS X 10.14.3 18D109, locale
    zh-Hans-CN)
[✓] Android toolchain - develop for Android devices (Android SDK version 28.0.3)
[✓] iOS toolchain - develop for iOS devices (Xcode 10.2)
[✓] Android Studio (version 3.3)
[✓] VS Code (version 1.32.3)
[✓] Connected device (1 available)
```

## 7. 新建flutter项目flutter Resolving dependencies...很慢
打开和新建flutter项目时发现，flutter Resolving dependencies...很慢。
### 解决办法：
打开Flutter SDK：flutter\packages\flutter_tools\gradle\flutter.gradle
改为以下即可解决！
```
buildscript {
    repositories {
        // google()
        // jcenter()
        maven{ url 'https://maven.aliyun.com/repository/google' }
        maven{ url 'https://maven.aliyun.com/repository/jcenter' }
        maven{url 'http://maven.aliyun.com/nexus/content/groups/public'}
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
    }
}
```
