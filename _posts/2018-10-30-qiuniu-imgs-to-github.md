---
title: 迁移七牛图床
tags: blog 云储存
key: 10036
date: 2018-10-30 00:08:00 +08:00
---

七牛对域名回收了，导致我的所有网络图片都需要转移，虽然我图片不多，但还是一件麻烦事，对七牛非常失望。

发牢骚也没用，我当然也理解七牛为什么要这样做，但还是要寻找解决办法。

我尝试绑定自己的域名，但由于自己的域名未备案，也根本来不及备案，只能作罢。

## 第一步，下载所有图片

使用七牛提供的 [qshell](https://developer.qiniu.com/kodo/tools/1302/qshell) 可以很快下载所有图片。

我是 windows 环境，即下载 qshell_windows_x64.exe 。

下载完成后，现在目录下创建一个配置文件，后面会用到。

```json
{
    "dest_dir"   :   "C:\\Users\\LEEWAY\\Downloads\\qiniu",
    "bucket"     :   "cloud9",
    "prefix"     :   "",
    "suffixes"   :   "",
    "cdn_domain" :   "ors3vio5q.bkt.clouddn.com",
    "referer"    :   "",
    "log_file"   :   "download.log",
    "log_level"  :   "info",
    "log_rotate" :   1,
    "log_stdout" :   false
}
```
只需要修改三个参数：

| 参数  | 内容  |
| ------------ | ------------ |
| "dest_dir"  | 下载文件保存的目录  |
| "bucket"  |  七牛储存空间名称 |
| "cdn_domain"  | 该储存空间的 cnd 域名  |

注：在 windows 环境中目录输入需要双斜杠。

接下来在 cmd 中输入指令即可。

```bash
qshell_windows_x64 account <AK> <SK> #登陆账户
qshell_windows_x64 qdownload qdownload.conf #执行下载
```

注：`<AK> <SK>`是自己的密钥 AccessKey 与 SecretKey，登陆七牛后，在[这里](https://portal.qiniu.com/user/key)查看

执行命令后即开始下载到指定目录：

![](http://k162.space/post_img/18-10-30/723215881.png)

## 第二步，尝试其他空间

梳理一下自己的需求：

1. 稳定
1. 转移方便
2. 便于管理
3. 费用低廉

### 1. github

还是用 github 吧，毕竟这些图片目前也就用来作为博客配图。

1. 在 github pages 的目录中建个文件夹，上传全部图片

2. 直接把全部博文中的`http://ors3vio5q.bkt.clouddn.com/`替换为`http://k162.space/post_img/`

### 2. 腾讯云

待折腾。

### 3. 其他

待折腾，求推荐。

## 参考

[jdhao's blog - 把博客图床从七牛云迁移到腾讯云](https://jdhao.github.io/2018/10/20/qiuniu_migrate_to_tencent_cos/)