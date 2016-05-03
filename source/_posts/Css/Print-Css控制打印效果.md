---
title: Print Css控制打印效果
tags:
  - Css
categories: Css
date: 2016-05-03 10:56:25
description: 显示器(screen)和打印机(printer)是两种差别很大的设备,所以从浏览器里看到的页面,打印出来也许和你看到的样子有很大的差距。因此如果要精确的控制打印效果就应该使用print css。
---

# 前言

不经过任何处理而直接打印网站上的页面会得到一个不理想的效果。显示器(screen)和打印机(printer)是两种差别很大的设备,所以从浏览器里看到的页面,打印出来也许和你看到的样子有很大的差距。因此如果要精确的控制打印效果就应该使用print css。接下来，我们来先了解一些语法。

# 语法

## @media  
* 描述: @media用于指定的设备类型使用不同的样式。
* 语法：@media  sMedia { sRules }
* 说明：sMedia：设备类型,sRules：样式表定义 
* 兼容性：IE5+（这里只是指CSS2，C33@media的功能更加强大，但是只有IE9+支持）

| 设备类型 |   CSS版本 | 兼容性 | 简介 |
| :-----|:----| :----| :----|
| all | CSS2 |   IE4+   | 用于所有设备类型 |
| aural  |  CSS2  |  NONE |   用于语音和音乐合成器 |
| braille  |CSS2  |  NONE  |  用于触觉反馈设备 |
| embossed   |  CSS2  |  NONE |   用于凸点字符（盲文）印刷设备 |
| handheld  |   CSS2  |  NONE  |  用于小型或手提设备 |
| print |   CSS2  |  IE4+  |  用于打印机 |
| projection  | CSS2  |  NONE  |  用于投影图像，如幻灯片 |
| screen  | CSS2  |  IE4+  |  用于计算机显示器 |
| tty | CSS2  |   NONE |   用于使用固定间距字符格的设备。如电传打字机和终端 |
| tv | CSS2   |  NONE   | 用于电视类设备 |

## @page 
* 描述: 用于指定打印页面的一些属性,包括纸张尺寸,方向,页边距,分页等特性。 
* 语法：@page  <label> <pseudo-classes>{ sRules }
* 说明：<label>：页面标识符 <pseudo-class>：打印伪类:first, :left, :right
* 兼容性：IE6+

# 使用

重新针对打印写CSS样式是没有必要的，我们只需要针对差异设置打印的样式覆盖掉之前的默认样式。

```bash
// 设置显示器用字体尺寸
@media screen {
body {font-size:12pt; }
}
// 设置打印机用字体尺寸
@media print {
body {font-size:8pt;}
}    
```

## 像素单位
 
 在编写打印样式表的时候，你要注意要使用厘米或者英寸作为单位而不是屏幕像素单位，实际的单位对打印非常有用。常见的DPI(Dot Per Inch)为96的screen,px与cm的换算关系如下：
* 1 inch = 2.54 cm

* 1cm = 96/2.54 ≈ 37.80 px

* 1px = 2.54/96 ≈ 0.0265 cm

* 100px = 2.65 cm

A4纸的标准尺寸为:

* 21.0cm * 29.7 cm

在96DPI分辨率下,其对应的像素尺寸大约为:

* 794px * 1123px

## 页边距
为了保证打印样式有用，写CSS样式使打印的内容距离纸张边缘，看起来更好，需要使用 @page 这个语法：

```bash
@media print {
   @page {
      margin: 2cm;
   }
}
```

## 换页打印 

* 为了保证不被跨页打印，如防止一个标题和内容在页面底部被分开：

```bash
h2, h3 {
 page-break-after: avoid; 
}
```

* 确保 articles 文章标签的内容能在新的一页开始：

```bash
article {
   page-break-before: always;
}
```

* 防止列表和图片分开在不同的页：

```bash
ul, img {
   page-break-inside: avoid;
}
```

* 属性介绍  

| 属性 | 描述 | CSS |
| :-----|:----| :----|
| page-break-after  |  设置元素后的分页行为 | 2 |
| page-break-before |  设置元素前的分页行为 | 2 |
| page-break-inside  | 设置元素内部的分页行为 |   2 |

| 值 |  描述 |
| :----- |:---- | 
| auto  |  默认。如果必要则在元素后插入分页符。 |
| always  | 在元素后插入分页符。在遇到特定的组件时，打印机会重新开始一个新的打印页 |
| avoid |  避免在元素后插入分页符。 |
| left   | 在元素之后足够的分页符，一直到一张空白的左页为止。 |
| right  | 在元素之后足够的分页符，一直到一张空白的右页为止。 |
| inherit | 规定应该从父元素继承 page-break-after 属性的设置。 |

## 背景图片和颜色 

如果用户是在 webkit 内核浏览器上打印的话，我们可以强制打印机打印屏幕上所看到的颜色（即强制在打印页面上出现任何的背景图和颜色），一般来说彩色打印机可以做到这点，我们需要一个单独的媒体查询：

```bash
@media print and (color) {
   * {
      -webkit-print-color-adjust: exact;
      print-color-adjust: exact;
   }
}
```

**注意:在firefox opera 和IE，不能使用背景图和颜色。**

## 其它

* 对于页面上有显示而不想打印的内容,可以将其display设置为none来避免打印。
* 需要打印的内容尽量避免float,有些浏览器不会正确的打印浮动的内容。
* 可以调用window.print()函数来打印当前页面。