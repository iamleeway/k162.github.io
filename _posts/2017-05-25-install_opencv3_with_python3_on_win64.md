---
layout: post
title: 安装 OpenCV 3.2.0(WIN64 python3 环境)
tags: python linux opencv
key: 10010
date: 2017-05-25 00:00:00 +08:00
---

更新时间：2017-05-25

目前，win64 python3 环境是无法直接安装 opencv 3.2.0 的；折腾一番，总算找到方法了，在此整理一下。

### 安装 opencv 3.2.0

1. 安装 [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=53587)
根据自己的需求选择 32位 或者 64位

1. 在 [http://www.lfd.uci.edu/~gohlke/pythonlibs/](http://www.lfd.uci.edu/~gohlke/pythonlibs/) 下载三方已经编译好的 whl 安装包，根据自己的需求选择对应版本（如果下载到的是zip文件可直接修改后缀为whl）：
- Numpy是必须的，这里我用的是：numpy-1.13.0rc2+mkl-cp35-cp35m-win_amd64.whl
- opencv：opencv_python-3.2.0-cp35-cp35m-win_amd64.whl

1. 打开CMD命令行窗口，切换到下载目录(可以直接在下载目录按住shift+右键打开命令行窗口)，执行pip安装

```bash
pip install numpy-1.13.0rc2+mkl-cp35-cp35m-win_amd64.whl
pip install opencv_python-3.2.0-cp35-cp35m-win_amd64.whl
```

以上就执行完毕了，可以打开python测试一下


```python
import cv2
print(cv2.__version__)
```

以上。

### 附：anaconda 直接安装 opencv 3.1.0
目前使用 conda 可直接安装 3.1.0 版本：

```bash
conda install -c menpo opencv3
```

一行命令搞定。

---

参考资料：https://www.solarianprogrammer.com/2016/09/17/install-opencv-3-with-python-3-on-windows/
