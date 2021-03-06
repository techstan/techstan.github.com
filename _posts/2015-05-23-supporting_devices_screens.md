---
layout: post
title: Android 屏幕适配
categories: 技术
tags: 屏幕适配
---

> **本博客为个人原创，转载需在明显位置注明出处**

&emsp;&emsp;1.在android开发中，常见的适配方案是：

			- 使用match_parent （100%参考父控件）
		    - 使用weight （按比例分配）
		    - 使用自定义View （按照百分比计算）
		    
	
  &emsp;&emsp;这些方案能解决大部分的适配问题，也可以看出来Android在对适配是引入了web同等方案，利用百分比的机制。
  
  
  &emsp;&emsp;在这里，还有一种方案，也是我认为比较好的适配方案。
  
  &emsp;&emsp;首先：<br/>
  	&emsp;&emsp;项目中针对你所需要适配的手机屏幕的分辨率各自简历一个文件夹
  	
  	values－ 480*320
  	values - 800*480
  	values - 960*540
  	... 
  	
  
  &emsp;&emsp;当我们要适配分辨率800*480 的手机时，
  
  &emsp;&emsp;在values-800*480 文件中建立lay_x.xml 也就是宽度对应的px的值
  那么你可能会问，lay_x.xml我要怎么针对这个800*480 来写对应的Px大小呢？
  
  &emsp;&emsp;我先写一个dimen_x.xml,看了大家就知道了。
  
  				<?xml version="1.0" encoding="utf-8" ?>
  				<resources>
    				<dimen name="x1">1.5px</dimen>
    				<dimen name="x2">3.0px</dimen>
  				</resources>
  
 &emsp;&emsp; 这里x1 的值指的是1dp中对应的像素值。（我是这么理解的，不知道有没有理解错误。）
  这个值是这么算的。
  
  x1 = 480 / 基准 = 480 / 320 = 1.5 ;
  
  &emsp;&emsp;然后后面依此乘以1.5.当然针对这么多的尺寸，我们也不可能一个一个算，还是有工具的。
  
  使用这样的好处是，当我要将按钮显示在屏幕的中央，我们可以这样去使用
  
  	android:layout_width="@dimen/x160"
  	android:layout_height="@dimen/x160"
  	...
  
  &emsp;&emsp;所以，当我们面对原型设计图，比如是480*320，我们就可以通过这种方式写出适配全分辨率的布局了。
  
 
  &emsp;&emsp;下面是自动生成适配文件的工具。就是计算每个分辨率下对应px的大小。<br/>
  &emsp;&emsp;https://github.com/techstan/generate_values
  
  ![适配工具](/images/shipeigongju.png)

  
  参考链接
  <a href="http://blog.csdn.net/i7788/article/details/44937277">开源，原创，实用Android 屏幕适配方案分享</a>
  
  
  
  
  
  
  