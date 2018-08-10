---
title: 使用 v2ray 科学上网
tags: linux v2ray
key: 10003
date: 2016-01-08 00:00:00 +08:00
---

2017-10-13 更新

## 一、简介
V2Ray 是一种类似SS的科学上网工具，采用独特的VMess协议进行加密传输，理论上更安全可靠。  
目前已支持 Windows、Mac OS X、Linux、Android。

- [官方手册](https://www.v2ray.com/)
- [github](https://github.com/v2ray/v2ray.github.io/wiki)

## 二、原理

![](http://ors3vio5q.bkt.clouddn.com/17-10-13/37308752.jpg)

运作方式与SS基本一致，通过加密协议与远程服务器连接绕过防火墙，因此同样需要配置服务器与客户端。

## 三、服务器端配置

环境：Debian

<!--more-->

### 1. 安装unzip

```bash
apt-get install unzip
```

### 2. 安装 V2Ray

```bash
curl -L -s https://raw.githubusercontent.com/v2ray/v2ray-core/master/release/install-release.sh | bash
```

即自动安装，并配置自动运行脚本
- V2Ray 安装目录: `/usr/bin/v2ray/`
- 配置文件目录: `/etc/v2ray/config.json`
- 日志文件: `/var/log/v2ray/access.log`
- 操作指令: `service v2ray start|stop|status|reload|restart|force-reload`

### 3. 配置服务器

编辑配置文件`vi /etc/v2ray/config.json`  
用过SS的童鞋一定很熟悉，同样只需要修改极少的参数

```json
{
  "port": 12286,  //端口，自己定义
  "log" : {
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log",
    "loglevel": "warning"
  },
  "inbound": {
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "2e29362e-b610-11e5-9f22-ba0be0483c18",   //用户id
          "level": 1
        }
      ]
    }
  }
```

用户id 后面类似密码的一行是UUID，可以通过[这个网站](https://www.uuidgenerator.net/)批量生成，配置多用户只要添加多行即可，例如：

```json
{
    "id": "2e29362e-b610-11e5-9f22-ba0be0483c18",
    "level": 1
},
{
    "id": "188b748e-b616-11e5-9f22-ba0be0483c18",
    "level": 1
},
{
    "id": "1d7d3ba8-b616-11e5-9f22-ba0be0483c18",
    "level": 1
}
```

### 4. 启动服务器
`service v2ray start`


## 四、配置客户端
### 1. 下载客户端
在[这里](https://github.com/v2ray/v2ray-core/releases)下载对应的客户端版本

- [win 32 v2.40](https://github.com/v2ray/v2ray-core/releases/download/v2.40/v2ray-windows-32.zip)
- [win 64 v2.40](https://github.com/v2ray/v2ray-core/releases/download/v2.40/v2ray-windows-64.zip)

### 2. 配置客户端
客户端只包含一个应用程序与一个配置文件，修改配置文件中的端口与id即可
例如：

```json
{
  "port": 6666,   //本地监听端口，自定义
  "log": {
    "access": ""
  },
  "inbound": {
    "protocol": "socks",
    "settings": {
      "auth": "noauth",
      "udp": false,
      "ip": "127.0.0.1"
    }
  },
  "outbound": {
    "protocol": "vmess",
    "settings": {
      "vnext": [
        {
          "address": "110.110.110.110",  //自己服务器的ip地址
          "port": 12286,  //服务器的端口，与客户端一致
          "users": [
            {"id": "2e29362e-b610-11e5-9f22-ba0be0483c18"}  //客户端id，与服务器端一致
          ]
        }
      ]
    }
  },
```

### 3. 运行`v2ray.exe`
后台静默启动可以直接运行`wv2ray.exe`

浏览器配置略，参见各种 SS 教程

## 五、总结
感觉不知名作者的无私奉献。捐助[作者](https://www.v2ray.com/#a=donate-zh-cn)请发送礼品卡编码至 donate@v2ray.com
自从SS作者被请喝茶以后众人人心惶惶，目测v2ray是个很好的替代方案。
因测试环境有限，目前还没有与SS的对比评测，以上。

### 如果实在不会配置
使用[配置生成器](https://htfy96.github.io/v2ray-config-gen/)吧:

![](http://ors3vio5q.bkt.clouddn.com/17-10-13/52183059.jpg)

### 备注
- 请**不要**写入本文所有配置文件中的注释部分，否则会报错
- v2ray 自带路由功能，可以自动判断是否走代理通道，不需要额外配置分流工具
- 详细配置请参考[配置文件](https://www.v2ray.com/chapter_02/)
