---
layout: post
title: MVC,MVP,MVVM初步
categories: 技术
tags: mvvm
---

前几天，Google I/O 2015 开始支持Data Binding（双向绑定），也称作为MVVM----

记得刚刚学开发的时候，只记得那时比流行MVC，这里来简单介绍一下MVC

	m----model  (模型：数据保存）
	v----view   (视图：用户界面)
	c----controller （控制器：业务逻辑）
	
	1.View 传送指令到 Controller
	2.Controller 完成业务逻辑后，要求 Model 改变状态
	3.Model 将新的数据发送到 View，用户得到反馈
	
这些通信都是单向的。

然后出现MVP的概念。
	MVP模型讲Controller改名为Presenter，同时也改变了通信方向。
	
		View－－－－－－－>Presenter－－－－－－－>Model
		View<－－－－－－－Presenter<－－－－－－－Model 
   
   	1. 各部分之间的通信，都是双向的。
	2. View 与 Model 不发生联系，都通过 Presenter 传递。
	3. View 非常薄，不部署任何业务逻辑，称为"被动视		图"（Passive View），即没有任何主动性，而 Presenter非常	厚，所有逻辑都部署在那里。
	
	            
然而又出现MVVM模式将 Presenter 改名为 ViewModel，基本上与 MVP 模式完全一致。
	
		view<--------->ViewModel-------------->Model
		view<--------->ViewModel<--------------Model
		
与MVP不一样的事，MVVM 采用的是双向绑定。View的变动，自动反映在 ViewModel

总结：感觉之前写代码，太偏向于软件的功能，觉得很多都揉在一起，使得在软件架构上很少去涉及，从而使得代码在后期维护上变得艰难，5.28号google io 2015 采用了双向绑定的（data-binding）让我认识了这个MVVM.

大家有机会去看一下。

参考资料
http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html

https://github.com/antoniolg/androidmvp

https://developer.android.com/tools/data-binding/guide.html

