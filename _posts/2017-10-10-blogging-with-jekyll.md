---
layout: post
title: 使用 jekyll 搭建博客
tags: blog tutorial
key: 10012
date: 2017-10-10 16:08:00 +08:00
---

博客再度搬迁了一下，一直以为小博客搭在自己服务器上很是惬意，直至经历了各种折腾后，深深体会到了老前辈们的真知灼见：把博客托管到三方，自己做到有限度的控制，才是最省心的，既然 github 提供了这么好的平台，那就索性玩一把，顺道记录分享一下吧。

### 用 jekyll 搭建有什么优势？
github 的 pages 服务是免费的，网站内容相当于托管给 github；而 jekyll 的作用是生成美美哒的静态网页。
这样做的优势是：
- **维护省心**，不用担心服务器挂掉、502、备份数据库、ddos等问题
- **markdown** 支持
- git 的工作流程科学又方便
- github pages **免费不限流量**
- 社区环境不错，学习成本也低

### 大致流程
1. github 上建立托管项目（就是网站目录）
    - 建立完即可访问网站
    - 可以使用自己的域名
1. 项目下载到本地，在本地进行编辑，写文章
    - jekyll 的作用是生成静态网页
    - 本地可以很方便的进行预览
    - 因此本地需要安装 jekyll 以及所依赖的环境
1. 完成编辑后上传更新项目即可
    - 刷新网页即可看到更新

## 一、github 设置
略，跟着 https://pages.github.com/ 官网步骤做即可。

- 注意创建项目的时候写全 `username.github.io`

## 二、本地设置
### 1. 安装 ruby
就说下 windows 环境：最新版安装完成后会自动安装必要组件，如果没有安装成功，可以打开cmd运行`ridk install`再次安装
注意 ridk install 安装包可能由于网络原因一直下载不成功，会自动重新下载，需要耐心

### 2. 安装 jekyll、bundle
```bash
gem install jekyll bundle
```

### 3. 下载项目到本地
`git clone https://github.com/username/username.github.io.git`

### 4. 运行服务
进入目录后：

```bash
bundle install # 只需运行一次
bundle exec jekyll server  # 运行服务
```

- 然后打开浏览器访问 127.0.0.1:4000 即可在本地预览网站

- 注意`bundle install`这个命令运行后会很长时间假死状态，疑似网络问题，一定要耐心等待

### 更新到 github
在本地修改好后，commit 然后 push 到 github 即可，关于 git 的使用方法略了。

## 三、定制化

### 使用个性化主题

[http://jekyllthemes.org/](http://jekyllthemes.org/) 有很多主题，看到中意的可以 git clone 下来，贴到自己的网站目录。

注意更换主题需要再次运行`bundle install`，因为主题所依赖的组件不同

- 自定义 css
主题的CSS文件中使用了很多变量，一般储存在`\_sass\variables.scss`，可以方便的修改页面的颜色

### 自定义域名
1. 购买域名，在域名解析页面设置 CNAME 到 username.github.io
1. 在网站目录下新建 CNAME 文件，写上自己的域名，注意不要填写 `http://`

### 统计浏览次数
我非常喜欢 TeXt 主题，这个主题已经配置好使用 LeanCloud 进行统计，简单写一下使用流程：

1. 注册 [LeanCloud](https://leancloud.cn)
1. 新建应用，应用名随意
1. 在应用面板：储存->创建 Class，名称随意，但要记住，后面要用
    注意 ACL 权限要设置为`无限制`，默认是`限制写入`
1. 在应用面板：设置->应用 Key，记录下`App ID`、`App Key`
1. 编辑主题的`_config.yml`
    设置 Reading quantity (LeanCloud)，填写刚才创建的`Class名称`、`App ID`、`App Key`