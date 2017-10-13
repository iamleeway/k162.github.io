---
layout: post
title: 使用 nginx 为网站添加身份验证
tags: vps linux nginx
key: 10004
date: 2016-02-04 00:00:00 +08:00
---

为了实现个人网站的私密访问，需要添加身份验证，即访问网站的时候要求输入用户名和密码，效果如下：

![](http://ors3vio5q.bkt.clouddn.com/17-10-13/75481446.jpg)

看了好多教程感觉略繁琐，其实使用nginx为网站添加身份验证登陆只需两个步骤：

1. 生成加密的验证信息文件
2. 修改网站的nginx配置文件

## 一、生成密码文件

nginx用以验证登陆的密码是以文件形式储存的，但并**非明文储存**，而是通过加密的，我们可以用`apache2-utils`工具的`htpasswd`命令生成

### 1. 安装工具

```bash
apt-get update
apt-get install apache2-utils
```

安装完毕即可使用`htpasswd`命令

### 2. 生成密码

生成`.htpasswd`文件，添加用户`user`，会要求输入密码两次：
(文件名、用户名、文件保存路径 均可以自行设置)

```bash
htpasswd -c /etc/nginx/.htpasswd user
```

若要添加多个用户，则去掉`-c`命令：

```bash
htpasswd /etc/nginx/.htpasswd another_user
```

查看已生成的密码文件`cat /etc/nginx/.htpasswd`
，格式为`用户名:密码`

## 二、添加验证

编辑网站配置文件，以 test.k162.cn 为例

```bash
vi /etc/nginx/conf.d/test.k162.cn.conf
```

在`location / {...}`句段添加：

```nginx
auth_basic "Restricted Content";  #提示文字，任意输入
auth_basic_user_file /etc/nginx/.htpasswd; #指向第一步生成的密码文件
```

重启nginx即可`service nginx restart`

## 参考
[How To Set Up Password Authentication with Nginx on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-14-04)
