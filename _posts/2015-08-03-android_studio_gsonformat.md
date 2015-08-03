---
layout: post
title: 使用GsonFormat插件快速生成实体类。
categories: 技术
tags: gsonformat
---


大多数服务端api都以json数据格式返回，然后我们客户端每次都要去写相应的实体了，有了这个工具，你将通过api接口生成相应的实体类。

 安装方法1:

	1.Android studio  File->Settings..->Plugins-->Browse repositores..搜索GsonFormat

	2.安装插件,重启android studio
	
 安装方法2:
 
 	1.下载GsonFormat.jar	
 	
    2.Android studio  File->Settings..->Plugins -->install plugin from disk..导入下载的GsonFormat.jar 
    
    3.重启android studio
    
 使用方式:
 	
 	创建实体类，右键－－－－>Generate－－－>GsonFormat－－－>弹出对话框，将获取到的gson数据拷贝到对话框中，确定之后，就将生成需要的实体了。
 	

但是要注意的是，此插件只适用于android studio和 Intellij IDEA 工具,eclipse 的少年请无视。



总结:今天只分享一些好用的工具，提升效率，而这次我也做了一次搬运工!
更多细则：https://github.com/zzz40500/GsonFormat