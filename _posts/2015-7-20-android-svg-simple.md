---
layout: post
title: android中使用svg资源文件
categories: 技术
tags: svg
---

&nbsp;&nbsp;&nbsp;&nbsp;从Android5.0开始就可以创建矢量的drawable,带来的最明显的优势是任意缩放不会变形，而且文件较小。

&nbsp;&nbsp;&nbsp;&nbsp;5.0新增了一个api－－－－－VectorDrawable 用于创建基于XML的矢量图，并结合AnimatedVectorDrawable来实现动画效果。

&nbsp;&nbsp;&nbsp;&nbsp;而且Android 5.0新增支持Vector标签，可以使用Path创建动画，同时支持SVG格式。

&nbsp;&nbsp;&nbsp;&nbsp;现在自己就写一个例子(参考网上的)  布局中定义一个ImageView

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/image_svg" />

    
然后在drawable中定义svg文件

	<?xml version="1.0" encoding="utf-8"?>
	<vector xmlns:android="http://schemas.android.com/apk/res/android"
    	android:height="256dp"
    	android:width="256dp"
    	android:viewportHeight="512"
    	android:viewportWidth="512">

    <path
        android:fillColor="#410a0a0a"
        android:pathData="@string/pretty_girl" />
	</vector>
	
	以下是string.xml部分
	
	<string name="pretty_girl">M 6.783868 184 C 6.783868 184 7.349524 160.57912 14.131572 157.31086 C 19.783036 155.677 29.389906 150.7756 31.085964 149.68606 C 32.78275 148.59634 32.78093 143.14954 32.78093 143.69422 C 32.78093 144.23854 20.34669 156.22168 11.870586 156.22168 C 29.9572 140.971 27.130012 136.06888 27.130012 136.06888 C 27.130012 136.06888 12.434786 147.50698 9.04267 146.41762 C 24.302642 132.80062 26.565084 126.26464 26.565084 126.26464 C 26.565084 126.26464 7.349524 136.06888 0 132.25594 C 17.523324 127.35418 22.044022 88.13767 22.044022 88.13767 C 22.044022 88.13767 25.998882 52.18906 28.82607 45.6529 C 29.9572 39.66178 41.26122 -2.27822 91.564273 4.80244 C 141.86809 11.88328 134.52148 75.06544 134.52148 76.69959 C 134.52148 78.333574 135.65042 76.15482 135.65042 81.056866 C 135.65042 85.95886 133.3898 88.13767 133.3898 88.13767 C 133.3898 88.13767 135.65133 93.584493 135.65133 102.84378 C 133.955276 101.20983 133.3898 98.486374 132.259764 100.12038 C 131.69538 106.656376 130.0006 105.022426 126.6083 117.54976 C 126.041916 124.0861 133.955276 128.98804 136.78028 128.98804 C 131.12918 131.16676 140.1713 132.80062 140.1713 132.80062 C 140.1713 132.80062 159.38686 135.52402 166.17091 139.88164 C 172.95351 144.23908 181.43398 169.29364 181.99854 182.91064 C 182.56402 183.99982 6.783868 184 6.783868 184 Z </string>
	
	
	
&nbsp;&nbsp;&nbsp;&nbsp;这里解释一下这几个属性； 
android:width=”256dp” 实际显示的宽度为256dp 
android:height=”256dp” 实际显示的高度为256dp 
android:viewportHeight=”512” 矢量限定的宽度为512，这没有单位； 
android:viewportWidth=”512” 矢量限定的高度为512 
其实以上两个属性，就是我们 svg 图片的宽高 

效果图如下：
	![适配工具](/images/svg.jpg)
	这个效果只有 Android5.0及之后的版本才支持，5.0以下需要第三方库支持(https://github.com/japgolly/svg-android)我也没去试，大家自行操作
	
&nbsp;&nbsp;&nbsp;&nbsp;总结:svg带来的好处是非常明显的，但是似乎还未流行起来，其带来的效率也非常明显，减少了android安装包的大小，也省去了ui设计以往的多套适配图片。据说svg可以通过VectorDrawable trimPathStart这个属性做动画，等有时间再进行实践吧！
