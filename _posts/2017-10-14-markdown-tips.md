---
layout: post
title: markdown 技巧分享
tags: tips
key: 10018
date: 2017-10-14 00:00:00 +08:00
---

> 关于介绍 markdown 书写规范的文章太多，一搜一大把，就不赘述了。
这里分享一些不常见但可能非常有用的技巧。

### 渲染格式问题：全角空格与半角空格

很多 Markdown 编辑器其实会自定义一些自己的书写规范来实现一些特殊功能，如果你已经习惯了一种编辑器，再跨到另一个编辑器上的时候可能就会遇到原有格式不能正确渲染的的问题。比如我习惯在为知笔记上用 editor.md 编辑器进行书写，书写的时候是不用考虑全角空格与半角空格的，当把文章复制到另一个编辑器（发布博客），就会遇到不能正确换行的问题，这其实就是空格键出了问题。

其实这个问题很好解决，用正则表达式对空格全部替换即可。

即：将 `(\S)\s` 替换成 `\1空格`

大概讲解下含义：
- `(\S)\s`意思是匹配 任意非空字符+空格 的字符串，无论是全角还是半角空格，都可以被`\s`匹配到
- `\1`表示刚才匹配到的第一个组，即保持括号内的内容不变，后面再加一个空格


### 时序图技巧：指定参与者顺序

大部分 markdown 编辑器都支持流程图和时序图，时序图也是非常实用的一种工具，如果参与者较多，不声明参与者顺序的情况，就会显得很混乱，比如这个描述 git 文件属性的时序图：

![](http://ors3vio5q.bkt.clouddn.com/17-10-14/14492462.jpg)

如果想要指定参与者顺序，需要先声明 `participant`:

```
participant untracked
participant unmodifed
participant modifed
participant staged
untracked->staged: add file
unmodifed->modifed: edit file
modifed->staged:stage file
staged->unmodifed: commit file
unmodifed->untracked: remove file
```

效果如下

![](http://ors3vio5q.bkt.clouddn.com/17-10-14/69026629.jpg)

### 快捷制作表格：从 EXCEL 或网页复制

使用 [http://www.tablesgenerator.com/markdown_tables](http://www.tablesgenerator.com/markdown_tables)

1. 贴入粘贴内容，可以从 EXCEL、网页、文本 等各种地方复制原表格
![](http://ors3vio5q.bkt.clouddn.com/17-10-13/38346715.jpg)

1. 确认是否需要修改，然后`Generate`
![](http://ors3vio5q.bkt.clouddn.com/17-10-13/40596074.jpg)

1. 拷贝到文本即可
![](http://ors3vio5q.bkt.clouddn.com/17-10-13/60633314.jpg)


| name       | age |
|------------|-----|
| LearnShare | 12  |
| Mike       | 32  |

### 合并单元格（html 表格）
markdown 的表格可能是出于简化考虑没有合并单元格这类功能，但实际又需要这项功能的时候怎么办呢？可以直接插入 html 表格

使用 [http://www.tablesgenerator.com/html_tables](http://www.tablesgenerator.com/html_tables)

- 注意勾选`Do not generate CSS`、`Compact mode`（为了让代码尽可能精简）

```html
<table><tr><th>class</th><th>name</th><th>age</th></tr><tr><td rowspan="2">one</td><td>LearnShare</td><td>12</td></tr><tr><td>Mike</td><td>32</td></tr></table>
```

<table><tr><th>class</th><th>name</th><th>age</th></tr><tr><td rowspan="2">one</td><td>LearnShare</td><td>12</td></tr><tr><td>Mike</td><td>32</td></tr></table>


### 多层级嵌套
一般很少用到超多层级的嵌套，直接打一个空格缩进可能无法正确渲染。

改用四个空格缩进吧：

```
1. 一级内容
    - 二级内容
        - 三级内容
            1. 四级内容
            1. 四级内容
1. 一级内容
    - ...
```

1. 一级内容
    - 二级内容
        - 三级内容
            1. 四级内容
            1. 四级内容
1. 一级内容
    - ...