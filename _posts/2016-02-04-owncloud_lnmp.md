---
title: lnmp 环境搭建 ownCloud 私有云
tags: vps linux nginx
key: 10005
date: 2016-02-04 00:00:00 +08:00
---

## 题记
在经历了两次心惊肉跳的硬盘数据恢复后，我渐渐把自己的数据资料转移到云端。与其惴惴不安地使用百度云，Dropbox等云储存工具，不如自己动手搭个云。

ownCloud是一个自由且开源的个人云存储解决方案，配置好服务器端后，就可以进行多平台的数据同步。

使用私有云之前必须先了解可能存在问题：
由于数据全部储存在服务器上，vps忘记续费导致被删除或者vps供应商跑路等状况将导致资料万劫不复，所以请谨慎操作，对数据资料**一定要做好备份**，永远不要把鸡蛋放一个篮子里！

本篇不探讨各种云储存方案的优劣，请自行斟酌。

## 安装步骤

搭建环境：Debian(Ubuntu)|nginx|mysql|php
省心的一键脚本可以戳这里：[Tennfy - debian下lnmp一键安装脚本完善版](http://www.tennfy.com/2123.html "debian下lnmp一键安装脚本完善版")
使用一键配置可以省略文本很多操作步骤，只需 8,9,11

<!--more-->

### 1. 安装 MySQL 5

```bash
apt-get install mysql-server mysql-client
```

会要求输入 mysql root 密码

### 2. 安装 Nginx

```bash
apt-get install nginx
```

### 3. 安装 PHP 5

```bash
apt-get install php5-fpm
```

编辑`/etc/php5/fpm/php.ini`
设置`cgi.fix_pathinfo=0`
重新载入php `service php5-fpm reload`

### 4. 让 PHP 支持 MySQL
安装一系列组件，一并安装 可选PHP缓存 `php-apc`
```
apt-get install php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps  php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl php-apc
```
重新载入php `service php5-fpm reload`

### 5. 让 PHP 使用 TCP 连接
默认情况下 PHP-FPM 监听的是 `/var/run/php5-fpm.sock`
编辑配置文件 `vi /etc/php5/fpm/pool.d/www.conf`
注释掉`listen = /var/run/php5-fpm.sock`修改为：

```php
;listen = /var/run/php5-fpm.sock
listen = 127.0.0.1:9000
```

重新载入php `service php5-fpm reload`

### 6. 安装 owncloud 所需组件
安装所需组件：

```sh
apt-get install openssl ssl-cert php5-cli php5-sqlite php5-common php5-cgi sqlite3 php-pear curl libapr1 libtool curl libcurl4-openssl-dev php-xml-parser php5 php5-dev php5-gd varnish
```

### 7. 创建SSL证书（一年有效期）
owncloud必须使用安全链接才可以访问，使用 openssl 证书浏览器会报不安全提示，建议使用 [免费SSL证书Let’s Encrypt](http://www.tennfy.com/3911.html) ，本文使用的是 openssl

```bash
openssl req $@ -new -x509 -days 365 -nodes -out /etc/nginx/cert.pem -keyout  /etc/nginx/cert.key
chmod 600 /etc/nginx/cert.pem
chmod 600 /etc/nginx/cert.key
```

### 8. 安装 owncloud

下载安装:

```sh
mkdir -p /var/www/owncloud # 创建网站目录
wget http://download.owncloud.org/community/owncloud-8.2.2.tar.bz2 #下载owncloud
tar xvf owncloud-8.2.2.tar.bz2 # 解压文件
mv owncloud/  /var/www/ # 将解压内容移至网站目录
chown -R www-data:www-data  /var/www # 添加nginx用户权限
sudo rm -rf owncloud owncloud-8.2.2.tar.bz2 # 删除压缩包
```

创建 owncloud 数据库，创建用户并赋予该数据库所有权限，不熟悉命令行的童鞋建议用 phpMyAdmin 操作，这次不再赘述。

```sh
mysqladmin -u root -p create owncloud
mysql -u root -p
```
```sql
GRANT ALL PRIVILEGES ON owncloud.* TO 'owncloud_admin'@'localhost' IDENTIFIED BY  'owncloud_admin_password'
GRANT ALL PRIVILEGES ON owncloud.* TO 'owncloud_admin'@'localhost.localdomain' IDENTIFIED BY 'owncloud_admin_password'
FLUSH PRIVILEGES
quit
```

### 9. 配置 nginx
编辑(创建)网站配置文件 `vi /etc/nginx/conf.d/owncloud.conf`
输入以下内容：

```nginx
server  {
    listen  80;
    server_name cloud.domain.com;  # 修改为自己的域名
    return  301  https://$server_name$request_uri;  # 强制使用 https
}
server  {
    listen 443  ssl;
    server_name cloud.domain.com;   # 修改为自己的域名
    # 证书及密钥位置
    ssl_certificate  /etc/nginx/owncloud.pem;
    ssl_certificate_key  /etc/nginx/owncloud.key;
    # 网站根目录
    root  /var/www/owncloud;
    client_max_body_size  16G;  # set max upload size
    fastcgi_buffers  64  4K;
    rewrite  ^/caldav(.*)$  /remote.php/caldav$1  redirect;
    rewrite  ^/carddav(.*)$  /remote.php/carddav$1  redirect;
    rewrite  ^/webdav(.*)$  /remote.php/webdav$1  redirect;
    index index.php;
    error_page  403  /core/templates/403.php;
    error_page  404  /core/templates/404.php;
    location  =  /robots.txt  {
        allow all;
        log_not_found off;
        access_log off;
    }
    location  ~  ^/(data|config|\.ht|db_structure\.xml|README)  {
            deny all;
    }
    location  /  {
            # The following 2 rules are only needed with webfinger
            rewrite  ^/.well-known/host-meta  /public.php?service=host-meta last;
            rewrite  ^/.well-known/host-meta.json  /public.php?service=host-meta-json last;
            rewrite  ^/.well-known/carddav  /remote.php/carddav/  redirect;
            rewrite  ^/.well-known/caldav  /remote.php/caldav/  redirect;
            rewrite  ^(/core/doc/[^\/]+/)$  $1/index.html;
            try_files  $uri  $uri/  index.php;
    }
    location  ~  ^(.+?\.php)(/.*)?$  {
            try_files  $1  =  404;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME  $document_root$1;
            fastcgi_param PATH_INFO  $2;
            fastcgi_param HTTPS on;
            fastcgi_pass  127.0.0.1:9000;
            # Or use unix-socket with 'fastcgi_pass unix:/var/run/php5-fpm.sock;'
    }
    # Optional: set long EXPIRES header on static assets
    location  ~*  ^.+\.(jpg|jpeg|gif|bmp|ico|png|css|js|swf)$  {
            expires  30d;
            # Optional: Don't log access to assets
            access_log off;
    }
}
```

### 10 .修改 PHP 传输限制
编辑配置文件 `/etc/php5/fpm/php.ini`
找到以下三项进行修改：

```php
upload_max_filesize  =  16G
post_max_size  =  16G
output_buffering  =  16384
```

在文件末尾，添加 apc 缓存配置信息

```php
upload_tmp_dir  =
extension  =  apc.so
apc.enabled  =  1
apc.include_once_override  =  0
apc.shm_size  =  256
```

重启 nginx `service nginx restart`
重启 php `service php5-fpm restart`

### 11. 配置完成

打开网址，即可看到 owncloud 初始页面
填写任意 用户名，密码 （创建访问账户）
输入在 步骤8 创建的owncloud数据库，数据库名户名及密码

若出现**502错误**，请将配置文件中的 `fastcgi_pass  127.0.0.1:9000;`注释掉，改为监听 `fastcgi_pass unix:/var/run/php5-fpm.sock;`

## 参考
[owncloud-debian-wheezy-derivates-ubuntu-using-nginx](https://computermen.linuxeverywhere.org/2014/02/11/owncloud-debian-wheezy-derivates-ubuntu-using-nginx/)