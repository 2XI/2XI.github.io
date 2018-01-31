---
title: What's the difference between a JavaScript framework and a library
date: 2018-02-01 00:12:18
tags:
---

What's the difference between a JavaScript framework and a library
==================================================================

1.  [Javascript library](#Javascript-library)
2.  [Toolkit](#Toolkit)
3.  [Javascript framework](#Javascript-framework)
4.  [Difference](#Difference)

### [](#Javascript-library "Javascript library")Javascript library

库的的概念和意义是用来提供一些方法的集合，避免重复定义相同功能的函数并具有一定的模式兼容性。库通常缺乏任何目的或意图的,并且旨在使用和集成客户端代码，协助客户端代码执行它的任务，常见的工具包如JQuery。

### [](#Toolkit "Toolkit")Toolkit

工具是一个有着特定目的的库，我的理解是它一般用来解决开发中的部分问题，或者说它提供了一个完整应用的单独的解决方案，比如说事件传递、控制流程或者网络通信等中的一个，常见的工具包如superagent。

### [](#Javascript-framework "Javascript framework")Javascript framework

Javascript framework可以用来组织大型应用，提供几乎完整的解决方案。对于大型前端框架来说需要提供这些解决方案：

*   数据层的抽象
    
*   视图的抽象
    
*   事件传递和控制流程
    
*   网络通信
    
*   路由管理
    

### [](#Difference "Difference")Difference

Javascript framework 和 Javascript library的区别再于：inversion of control，也就是：

your code calls a library but a framework calls your code.

意思是：

*   当你在调用library的时候， 你按照自己的意愿来control他（比如jQuery）。
    
*   对于framework， 那么control就是倒转过来了，是他在调用你（比如Vue), 就像是Hollywood的一个principle: Don’t call Us, We’ll call You.
    

在使用framework的时候，自定义代码仅仅关注处理事件本身，由event loop 和 dispatch的events/messages机制来进行事件调用，它们是由framework或者运行时环境提供的。

Inversion of Control是一个framework和一个library的根本区别，library从本质上来讲是一组你可以调用的函数，这些通常组织成类，每次调用做些处理然后将控制权返还给客户端。一个框架包含了一些抽象的设计，内置更多的钩子，为了使用这些钩子，你需你需要通过将框架子类化或者在这些地方插入自己的类。然后框架会在这些地方调用你的自定义代码。