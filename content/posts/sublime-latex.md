---
title: "Sublime编辑LaTeX的方法总结"
date: 2019-09-17T22:59:39+08:00
draft: false
categories: ["Tools"]
tags: ["Latex", "Sublime"]
---

# Sublime编辑LaTeX的方法总结


## Windows下sublime+samutraPDF工具链

windows下一般以sublime编辑latex文件，并用sumatrapdf来查看pdf和反向跳转到latex源码。

windows下一般通过Ctex套件来安装latex相关编译工具，安装Ctex完毕后`pdflatex` `xelatex` `bibtex`等工具应已经全部就绪。

### 安装latexTools

安装sublime，通过package control安装`LatexTools`。

> Preference -> Package Cotnrol
> Install New Packages
> LatexTools

LatexTools在windows下一般使用pdflatex编译tex文件，因此如果原文件中使用了eps格式的图片，一般需要在引言部分加上
```tex
\usepackage{graphicx}
\usepackage{epstopdf}
```
将eps图片转为pdf格式图片，使得pdflatex支持。

### 配置LatexTools

Windows下`LatexTools`默认的查看pdf的viewer是[sumatrapdf](https://www.sumatrapdfreader.org/free-pdf-reader.html)。安装完毕后配置User配置文件，主要是配置viewer的路径，
windows下默认的viewer一般为sumatraPDF。
```json
	"windows": {
		// Command to invoke Sumatra. If blank, "SumatraPDF.exe" is used (it has to be on your PATH)
		"sumatra": "D:\\Application\\SumatraPDF\\SumatraPDF.exe",
	},
```

### 配置sumatraPDF

打开sumatrapdf，在设置选项中加上“双击pdf时执行命令”
```bash
"D:\Application\Sublime Text 3\sublime_text.exe" "%f:%l"
```
以此实现双击反向跳转。配置完成之后，在latex文件中使用`Ctrl+B`可以编译latex文件，使用`Ctrl+l v`可以预览编译好的pdf文件。在sumatrapdf中双击对应文字，可以跳转回latex源码中。

**对于包含多tex文件的文章**，可以在每一个子文件开头加上
```tex
%!TEX root = XXX.tex
```
以保证在各个子文件中均可以编译和查看pdf文件。

## Mac下实现sublime+skim工具链

Mac下一般以sublime编辑latex文件，并用skim来查看pdf和反向跳转到latex源码。

Mac首先安装MacTex相关套件。安装完毕后，有可能会出现path路径找不到编译套件的情况。此时需要修改`/etc/paths`文件加上mactex套件的路径。

按照windows下相同的方法下载`LatexTools`。

### 配置skim

Mac下`LatexTools`默认的查看pdf的viewer是skim。
安装[skim](http://skim-app.sourceforge.net/)，选择选项->同步，并设置pdf-tex查看软件为sublime text 2。

此时打开终端，输入`subl`如果不能打开sublime，则需要链接`subl`到`PATH`目录下。

```bash
sudo ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/bin/subl
```

完成后再skim中按住`Command+Shift`再点击对应的文字，就会跳转到相应的源代码。

