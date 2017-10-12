---
layout: post
title: Ubuntu16.04 编译安装为知笔记
tags: linux
key: post
category: linux
date: 2017-06-01 00:00:00 +08:00
---

为知笔记linux客户端的安装不是很便捷，最新版本需要自己动手编译。按照官方教程，安装过程中会遇到各种问题，相信经历这番折腾的不止我一个人，一并整理一下吧：

- 为知笔记版本：v2.5.1

### 1. 安装环境
安装所需的相关工具

```bash
sudo apt-get install build-essential
sudo apt-get install cmake
sudo apt-get install zlib1g-dev
sudo apt-get install mesa-common-dev
sudo apt-get install libglu1-mesa-dev -y
sudo apt-get install build-essential libgl1-mesa-dev -y
```

### 2. 安装 QtCreator
下载 QtCreator

```bash
wget http://download.qt.io/official_releases/qt/5.7/5.7.0/qt-opensource-linux-x64-5.7.0.run
```

接下来是图形化安装界面，略
![](http://ors3vio5q.bkt.clouddn.com/17-10-12/92330134.jpg)

#### 2.1 添加 fcitx 的qt支持
非常重要，若不添加，会造成无法切换输入法问题

```bash
sudo apt install extra-cmake-modules libxkbcommon-dev
sudo apt-get install bison
wget https://github.com/fcitx/fcitx-qt5/archive/1.1.0.zip
unzip -q 1.1.0.zip
cd fcitx-qt5-1.1.0
cmake .
make
sudo make install
```

完成编译之后，把 `citx-qt5/build/`下的 `platforminputcontext/libfcitxplatforminputcontextplugin.so` 复制到Qt安装目录内，包含2个目录，分别是：

1. Qt5.7.0/5.7/gcc_64/plugins/platforminputcontexts/
2. Qt5.7.0/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts/

### 3. 下载为知笔记源码

```bash
wget https://github.com/WizTeam/WizQTClient/archive/v2.5.1.zip
unzip -q v2.5.1.zip
```

### 4. 进行编译
打开 QtCreator，打开文件，选择为知目录下的`CMakeLists.txt`文件
![](http://ors3vio5q.bkt.clouddn.com/17-10-12/66978372.jpg)
点击`Configure Project`
![](http://ors3vio5q.bkt.clouddn.com/17-10-12/828240.jpg)
然后直接`Run CMake`运行即可，在编译输出栏可以看到进度，可能会看到这个错误：
![](http://ors3vio5q.bkt.clouddn.com/17-10-12/59326021.jpg)
这是由于使用了高版本的`GCC/G++ 5`（Ubuntu16默认的版本），若使用低版本的就不会报错，如降级为`GCC/G++ V4.8`。
编译完成之后就可以运行wiz客户端了。

### 参考
http://www.jjbiji.com/?p=151
http://www.wiz.cn/compile-client.html
https://my.oschina.net/lieefu/blog/505363