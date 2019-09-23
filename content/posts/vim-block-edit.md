---
title: "Vim Block Edit"
date: 2019-09-17T20:34:19+08:00
draft: false
categories: ["Tools"]
tags: ["Vim"]
---

# Vim 块编辑方法

## 末尾添加字符

```
<a href="Slides/01Introduction-17.ppt">Lecture 01</a><br/>
<a href="Slides/02Performance-17.ppt">Lecture 02</a><br/>
<a href="Slides/03ISA-17.ppt">Lecture 03</a><br/>
<a href="Slides/04singlecycle.ppt">Lecture 04</a><br/>
```

1. 命令模式按`ctrl+v`进入纵向编辑模式，将光标移至`Slides`末尾，向下移动光标，选中4行的`Slides`末尾的字符。
2. 然后按`A`进入**行尾插入模式**，添加2017。
3. 按`ESC`退出行尾编辑模式，此时4行`Slides`均变为`Slides2017`。

```
<a href="Slides2017/01Introduction-17.ppt">Lecture 01</a><br/>
<a href="Slides2017/02Performance-17.ppt">Lecture 02</a><br/>
<a href="Slides2017/03ISA-17.ppt">Lecture 03</a><br/>
<a href="Slides2017/04singlecycle.ppt">Lecture 04</a><br/>
```