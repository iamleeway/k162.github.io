---
layout: post
title: 使用jekyll在github搭建博客
date: 2017-10-09 22:47:00.000000000 +08:00
---

## 原理
github 的 pages 服务是免费的，网站内容相当于托管给 github；而 jekyll 的作用是生成美美哒的静态网页。
这样做的优势是：
- 维护省心，不用担心服务器挂掉、备份数据库、ddos攻击等运维问题
- markdown 支持
- git 的工作流程比较科学
- 免费不限流量
- 社区环境不错，学习成本也低

## github 设置
跟着 https://pages.github.com/ 官网步骤做即可。

## 本地环境搭建
1. 安装 ruby
windows 环境下，最新版安装完成后会自动安装必要组件，如果没有安装成功，可以打开cmd运行`ridk install`再次安装
注意 ridk install 安装包可能由于网络原因一直下载不成功，会自动重新下载，需要耐心

1. 安装 jekyll、bundle
```bash
gem install jekyll bundle
```

1. 然后再运行 `bundle install` 安装一些其他必要的东西
注意这个命令运行后会很长时间假死状态，疑似网络问题，一定要耐心等待

1. 运行服务
```bash
git clone https://github.com/username/username.github.io.git  # git自己的网站到本地
cd k162.github.io.git  # 进入网站目录
bundle exec jekyll serve  # 运行服务
```
然后打开浏览器 127.0.0.1:4000 即可。

## 自定义样式
### 使用主题
http://jekyllthemes.org/ 有很多主题，看到中意的可以 git 下来，贴到自己的网站目录即可

### 自定义css颜色
这个主题的CSS文件中使用了很多变量，这里修改`\_sass\variables.scss`，可以方便的修改所有用到该变量的页面css颜色

## 自定义域名
1. 购买域名，在DNS服务商那边设置 CNAME 到 username.github.io

1. 在网站目录下新建 CNAME 文件，写上自己的域名 blog.k162.space