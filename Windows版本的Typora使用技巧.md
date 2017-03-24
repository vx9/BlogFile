---
typora-copy-images-to: images
typora-root-url: ./images
---

[TOC]

![typora](images/typora.png)

# Windows版本的Typora使用技巧#

## 前言##

Typora是一款轻便简洁的Markdown编辑器，支持即时渲染技术，这也是与其他Markdown编辑器最显著的区别。即时渲染使得你写Markdown就想是写Word文档一样流畅自如，不像其他编辑器的有编辑栏和显示栏。

其他优点也有很多，比如你可以使用右键给文字加上链接，或者插入一个表格等，右键你会发现有很多方法，你可以立马试试。

本文将以自己 ***写MD文件经常会遇到的问题***  为基本出发点编写。

> Markdown的详细的基本语法不做介绍，如需了解请看[typora在线文档](http://support.typora.io/Markdown-Reference/)。

### 1.如何显示文章大纲目录？###

```
方法：[toc]
有些编辑器不支持，github上不支持。CSDN上支持。
```

前提：你的文章按照H1-H6这种大纲书写。

作用：用来生成文章的大纲目录

位置：哪里都可以，不一定放在最前面。[本文讨论的typora的版本:0.9.23(beta)]

例子：就如同本文前面那样。

### 2.如何改变已写文字的样式？###

问题场景：本来要写三级标题，结果多写了一个#，变成四级标题怎么办？

问题分析：如果你去掉#（或者重新加上#），你发现很别扭。

方法：

- 方法一：使用鼠标右键

  选择文字，右键，选择`Paragraph`，选择`Heading 3`.

- 方法二：使用快捷键

  选择文字，ctrl+3

- 方法三：使用工具栏

  选择文字，工具栏选择`Paragraph`，选择`Heading 3`.

  （就像在Word一样）

问题总结：这个问题可以延伸很多情景，比如普通文字变成链接文字、增加或减少文字的缩进等，这里你会发现：真的跟word是兄妹。

### 3.如何插入表格？###

如同“2.如何给标题降级”一样，用鼠标右键，快捷键，工具栏，你都可以找到方法，当然你也可以用最基本的语法。

### 4.能否像word一样将文字拖拽到某个位置？

抱歉，不能。至少目前我没有发现，如果你知道，请告诉我。

### 5.如何让插入图片变得轻松些？###

1. 问题场景：如果从剪贴板得到的图片能复制到文章（拖拽图片），而且能够自动保存在某一位置就好了。

   步骤一：`File`-->`Preference`-->`Editor`-->`Image Insert`-->`Allow copy images to given folder` 

   步骤二：`Edit`-->`ImageTools`-->`When Insert Local Images`--> `Copy Image File to Folder...`

   > 以上步骤目的：在编辑文章时从剪贴板获得的图片或者拖拽图片，自动会将该图片存储到指定文件夹下。步骤二会出现文件夹选择框，选择你将要保存图片文件的文件夹位置。

   > TIP：图片支持拖拽，但是图片不支持粘贴复制，但是却可以从剪贴板中粘贴，不知道为什么。

2. 问题场景：如果拖拽的图片，粘贴的图片自动使用相对路径，就可以方便上传到GitHub了。

   > 将图片与文章放在同一个文件夹下，方便使用相对路径表示图片。

   方法一：`File`-->`Preference`-->`Editor`-->`Image Insert`-->`Allow relative path if possible` 

   方法二：`Edit`-->`ImageTools`-->`When Insert Local Images`--> `Use Image Root Path`

3. 问题场景：轻松发布到别的地方，比如CSDN博客？

   > 没有很好的方法，有人推荐用MWeb+一系列工具，但是MWeb收费。我就先上传到GitHub，然后就手动给图片加上路径。



