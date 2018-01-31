---
title: line－height的作用对象
date: 2018-01-31 23:59:24
tags:
---

line－height的作用对象
================

1.  [line-height作用于行内元素或者原生的行内块级元素](#line-height作用于行内元素或者原生的行内块级元素)
2.  [步入主题：](#步入主题：)

### [](#line-height作用于行内元素或者原生的行内块级元素 "line-height作用于行内元素或者原生的行内块级元素")line-height作用于行内元素或者原生的行内块级元素

* * *

这个文章设及到继承和line-height的相关知识:  
阅读中遇到问题可以回来参考：[css继承](http://www.cnphp.info/css-style-inheritance.html)和[line-height之w3c](http://www.w3school.com.cn/cssref/pr_dim_line-height.asp)

### [](#步入主题： "步入主题：")步入主题：

* * *

先来看个例子：

为什么box这个div包住了cont而没有包住input呢？首先我们要明确line-height只对行内元素或者原生的行内块级元素有效。当我们尝试把cont的显示方式改为inline-block发现显示cont并没有改变位置，说明它对非原生行内块级元素无效。box中的cont是块级元素会正常显示，但是由于cont继承了box的行高属性值，所以元素会把300像素作为自己的行高，从cont的左上角开始计算自己的显示位置，就会出现文章开始出现的情况。