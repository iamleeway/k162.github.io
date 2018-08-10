---
title: 一键切换浏览器 chrome firefox edge
tags: autohotkey
key: 10006
date: 2016-08-04 00:00:00 +08:00
---

Chrome 与 Firefox 都是非常优秀的浏览器，出于有些需要，我经常要将当前标签页发送到另一个浏览器打开。chrome 应用商店内有些扩展插件可以实现，但反响都一般，这些扩展需要访问网页信息，出于对安全的忧虑，就想到了是不是可以用 Autohotkey 来实现呢，摸索一番，发现非常简单。

这个 Autohotkey 脚本能实现以下功能

1. 按`Alt+Shift+F`将标签页发送到 Firefox
1. 按`Alt+Shift+C`将标签页发送至 Chrome
1. 按`Alt+Shift+E`将标签页发送至 edge

<!--more-->

安装 Autohotkey 后，新建文本文件，写入以下代码，存为`.ahk`文件，直接运行即可。
如需自定义快捷键，请修改对应热键代码。

```
;Alt+shift+F 发送标签至 Firefox
!+f::
send ^l   ;定位到浏览器地址栏
sleep 288  ;设置一点点延时缓冲时间
send ^c  ;复制当页面地址
send ^c  ;针对edge的特殊情况需复制url两次
URL = %clipboard%  ;将剪贴板储存到变量
FoundPos := RegExMatch(URL, "http") ;判断字符串是否为URL
if FoundPos = 1
	run firefox.exe %URL%  ;运行firefox
else
	run firefox.exe
	return

;Alt+shift+C 发送标签至Chrome
!+c::
send ^l
sleep 288
send ^c
send ^c
URL = %clipboard%
FoundPos := RegExMatch(URL, "http")
if FoundPos = 1
	run chrome.exe %URL%
else
	run chrome.exe
	return

;Alt+shift+E 发送标签至 Edge
!+e::
send ^l
sleep 288
send ^c
send ^c
URL = %clipboard%
FoundPos := RegExMatch(URL, "http")
if FoundPos = 1
{
	run %windir%\explorer.exe shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge
	sleep 1000
	send ^l
    sleep 180
	send ^v
	send {enter}
	sleep 180
	send {enter}
}
else
	run %windir%\explorer.exe shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge
	return
```