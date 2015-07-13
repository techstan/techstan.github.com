---
layout: post
title: 优化AndroidStudio/Gradle构建
categories: 技术
tags: androidStudio
---
> **本博客部分为个人原创，转载需在明显位置注明出处**


&nbsp;&nbsp;&nbsp;&nbsp;使用AndroidStudio也有一段时间了，前一段时间打算出个AndroidStudio使用教程，但是看到网上很多关于这方面的资料，就决定不再重复造轮子了。讲点实际的，优化AndroidStudio,加快Gradle构建。

&nbsp;&nbsp;&nbsp;&nbsp;首先你得有一台电脑，然后你还得安装了AndroidStudio,不然你优化个毛线。（好了，不扯了，勿喷）

&nbsp;&nbsp;&nbsp;&nbsp;网上也有一些关于优化的资料，实际开发中也有明显的优化效果，先列举网上常见的优化技巧。

&nbsp;&nbsp;&nbsp;&nbsp;1.开启Gradle单独的守护进程，就是当电脑配置比较低的情况下，对程序进行编译，AndroidStudio容易崩掉，那么另外一个进行会及时的把你救活，免去了重新开启AndroidStudio的麻烦。

具体配置如下。

	/home/<username>/.gradle/ (Linux)
	/Users/<username>/.gradle/ (Mac)
	C:\Users\<username>\.gradle (Windows)
	
把下面配置复制gradle.properties文件也可以优化：

	# Project-wide Gradle settings.
	# IDE (e.g. Android Studio) users:
	# Settings specified in this file will override any Gradle settings
	# configured through the IDE.
	# For more details on how to configure your build environment visit
	# http://www.gradle.org/docs/current/userguide/build_environment.html
	# The Gradle daemon aims to improve the startup and execution time of Gradle.
	# When set to true the Gradle daemon is to run the build.
	# TODO: disable daemon on CI, since builds should be clean and reliable on servers
	org.gradle.daemon=true
	# Specifies the JVM arguments used for the daemon process.
	# The setting is particularly useful for tweaking memory settings.
	# Default value: -Xmx10248m -XX:MaxPermSize=256m
	org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
	# When configured, Gradle will run in incubating parallel mode.
	# This option should only be used with decoupled projects. More 	details, visit
	# http://www.gradle.org/docs/current/userguide/	multi_project_builds.html#sec:decoupled_projects
	org.gradle.parallel=true
	# Enables new incubating mode that makes Gradle selective when 	configuring projects. 
	# Only relevant projects are configured which results in faster 	builds for large multi-projects.
	# http://www.gradle.org/docs/current/userguide/	multi_project_builds.html#sec:configuration_on_demand
	org.gradle.configureondemand=true
	
	
&nbsp;&nbsp;&nbsp;&nbsp;同时上面的这些参数也可以配置到前面的用户目录下的gradle.properties文件里，那样就不是针对一个项目生效，
而是针对所有项目生效。上面的配置文件主要就是做， 增大gradle运行的java虚拟机的大小，让gradle在编译的时候使用独立进程，让gradle可以平行的运行。

&nbsp;&nbsp;&nbsp;&nbsp;2.申请大内存，这个方法和以前配置eclipse方案差不多。找到Android的安装目录，
installation path\studio64.exe.vmoptions or studio.exe.vmoptions

使用文本编辑器打开，找到起始两行，如下

	-Xms128m
	-Xmx750m

修改最小值和最大值，建议为

	-Xms256m
	-Xmx2048m
	
&nbsp;&nbsp;&nbsp;&nbsp;3.优化编译

    file->setting->compile

    勾选除第二项之外的其他选项，并在VM options里填入：

    -Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

&nbsp;&nbsp;&nbsp;&nbsp;4.快速导入github 开源项目。

&nbsp;&nbsp;&nbsp;&nbsp;前段时间，发现在每次导入项目的时候，非常卡，gradle一直在构建。刚开始怀疑是网络的原因，后来才知道，这是因为开源项目和本地Gradle版本不一致导致的。（因为版本不一致，导致每一次都要重新下载不同版本的Gradle）找到了问题的，就有了好的解决办法了。
	
	1.首先找到工程目录中gradle包下的gradle-wrapper.properties 这个文件记录gradle版本信息。
	将distributionUri改为当前版本的gradle
	
	2.其次还要在工程根目录下build.gradle中对其进行更改。主要更改dependencies
	dependencies {
        classpath 'com.android.tools.build:gradle:1.2.3'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
    
&nbsp;&nbsp;&nbsp;&nbsp;这样再次导入，速度上就非常明显了。
   
&nbsp;&nbsp;&nbsp;&nbsp;总结:之前一直使用eclipse,中间也尝试过使用AndroidStudio,但是当时由于AndroidStudio不稳定，再加上引入了gralde和module的概念。导致仅仅在运行Demo的时候才会用到AndroidStudio.随着Android版本越来越稳定，出现的问题也越来越少，现在已经在工作环境完全迁移到AndroidStudio中了。比起eclipse不知道好用多少倍，加上第三方插件的使用，使得开发效率上得到提高。所以推荐还没使用Android的童鞋们，要赶紧使用起来吧，一旦你尝到了甜头，你就回不去了。。😄😄
    
	
