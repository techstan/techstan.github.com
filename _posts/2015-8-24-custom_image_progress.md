---
layout: post
title: android自定义图片进度展示
categories: 技术
tags: progress
---

&nbsp;&nbsp;&nbsp;&nbsp;前些天项目中，需要以一个图片的形式来展示进度，想了想，发现以前有看到过类似的。主要是使用ClipDrawabled

![图片进度](/images/progress.gif)

&nbsp;&nbsp;&nbsp;&nbsp;现在我们一步一步的去实现它。
1.首先我们在xml布局中定义一个ImageView。

	<ImageView xmlns:android="http://schemas.android.com/apk/res/android"
           android:id="@+id/clothes_progress"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:layout_gravity="center"
           android:background="@mipmap/clothes_icon"
           android:scaleType="centerInside"
           android:src="@drawable/clip_loading"
    />
&nbsp;&nbsp;&nbsp;&nbsp;这里要注意的是，在指定src文件的时候，需要对其处理。
为了能够展示进度，我们需要用到clip标签
我们再进入到custom_progress.xml文件中。
    
    <?xml version="1.0" encoding="utf-8"?>
    <clip xmlns:android="http://schemas.android.com/apk/res/android"
    	android:clipOrientation="vertical"
    	android:drawable="@mipmap/clothes_progress"
    	android:gravity="bottom" >
    </clip>
    
其中

	android:drawable：指定截取的源Drawable对象
	android:clipOrientation：指定截取的方向，可设置为水平截取或垂直截取
	android:gravity：指定截取时的对齐方式
	
&nbsp;&nbsp;&nbsp;&nbsp;2.最后自定义一个view，关键代码如下。
		
	View view = LayoutInflater.from(context).inflate(R.layout.custom_progress, null);
	addView(view);
	ImageView imageView = (ImageView) findViewById(R.id.clothes_progress);
	mClipDrawable = (ClipDrawable) imageView.getDrawable();

&nbsp;&nbsp;&nbsp;&nbsp;代码中我们可以看到

	mClipDrawable = (ClipDrawable) imageView.getDrawable();

&nbsp;&nbsp;&nbsp;&nbsp;意思是将获得的资源文件转成ClipDrawable.

&nbsp;&nbsp;&nbsp;&nbsp;还没完，到了这一步，图片也许仍然没有什么效果，这时我们需要对指定setLevel(int level)方法来设置截取的区域大小，当level为0时，截取的图片片段为空；当level为10000时，截取整张图片。
这样通过循环的方式不断的对setLevel 值设置，就可以产生一个类似于进度的效果。（具体可参照源码）
https://github.com/techstan/CustomImageProgress




       