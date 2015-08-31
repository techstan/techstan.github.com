---
layout: post
title: android中使用proguard混淆代码
categories: 技术
tags: proguard
---
	
&nbsp;&nbsp;&nbsp;&nbsp;上周开发一个新项目，又要经历打包，混淆的过程，所以就干脆总结一下，也方便以后查阅，
在android studio中，


	release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'),'proguard-rules.pro'
        }
        
&nbsp;&nbsp;&nbsp;&nbsp;和eclipse有一点不样的是这里将混淆配置写在proguard-rules.pro文件中, 并且minifyenabled为true.

&nbsp;&nbsp;&nbsp;&nbsp;以下就是我的所有配置信息.

    #指定代码的压缩级别
    -optimizationpasses 5
     
    #包明不混合大小写
    -dontusemixedcaseclassnames
     
    #不去忽略非公共的库类
    -dontskipnonpubliclibraryclasses
     
     #优化  不优化输入的类文件
    -dontoptimize
     
     #预校验
    -dontpreverify
     
     #混淆时是否记录日志
    -verbose
     
     # 混淆时所采用的算法
    -optimizations !code/simplification/arithmetic,!field/*,!class/merging/*
     
    #保护注解
    -keepattributes *Annotation*
     
    # 保持哪些类不被混淆
    -keep public class * extends android.app.Fragment
    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider
    -keep public class * extends android.app.backup.BackupAgentHelper
    -keep public class * extends android.preference.Preference
    -keep public class com.android.vending.licensing.ILicensingService
    #如果有引用v4包可以添加下面这行
    -keep public class * extends android.support.v4.app.Fragment
     
     
    #忽略警告
    -ignorewarning
     
    #####################记录生成的日志数据,gradle build时在本项目根目录输出################
     
    #apk 包内所有 class 的内部结构
    -dump class_files.txt
    #未混淆的类和成员
    -printseeds seeds.txt
    #列出从 apk 中删除的代码
    -printusage unused.txt
    #混淆前后的映射
    -printmapping mapping.txt
     
    #####################记录生成的日志数据，gradle build时 在本项目根目录输出-end################
        
    #如果引用了v4或者v7包
    -dontwarn android.support.**
     
    ############<span></span>混淆保护自己项目的部分代码以及引用的第三方jar包library-end##################
    #gson
    -keepattributes *Annotation*
    -keep class sun.misc.Unsafe { *; }
    -keep class com.idea.fifaalarmclock.entity.***
    -keep class com.google.gson.stream.** { *; }
    
    #javabean不能被混淆
    -keep class com.xxxx.android.base.entity.** { *; }
     
     
    -keep public class * extends android.view.View {
        public <init>(android.content.Context);
        public <init>(android.content.Context, android.util.AttributeSet);
        public <init>(android.content.Context, android.util.AttributeSet, int);
        public void set*(...);
    }
     
    #保持 native 方法不被混淆
    -keepclasseswithmembernames class * {
        native <methods>;
    }
     
    #保持自定义控件类不被混淆
    -keepclasseswithmembers class * {
        public <init>(android.content.Context, android.util.AttributeSet);
    }
     
    #保持自定义控件类不被混淆
    -keepclasseswithmembers class * {
        public <init>(android.content.Context, android.util.AttributeSet, int);
    }
    #保持自定义控件类不被混淆
    -keepclassmembers class * extends android.app.Activity {
       public void *(android.view.View);
    }
     
    #保持 Parcelable 不被混淆
    -keep class * implements android.os.Parcelable {
      public static final android.os.Parcelable$Creator *;
    }
     
    #保持 Serializable 不被混淆
    -keepnames class * implements java.io.Serializable
     
    #保持 Serializable 不被混淆并且enum 类也不被混淆
    -keepclassmembers class * implements java.io.Serializable {
        static final long serialVersionUID;
        private static final java.io.ObjectStreamField[] serialPersistentFields;
        private void writeObject(java.io.ObjectOutputStream);
        private void readObject(java.io.ObjectInputStream);
        java.lang.Object writeReplace();
        java.lang.Object readResolve();
    }
     
    #保持枚举 enum 类不被混淆 如果混淆报错，建议直接使用上面的 -keepclassmembers class * implements java.io.Serializable即可
    #-keepclassmembers enum * {
    #  public static **[] values();
    #  public static ** valueOf(java.lang.String);
    #}
     
    -keepclassmembers class * {
        public void *ButtonClicked(android.view.View);
    }
     
    #不混淆资源类
    -keepclassmembers class **.R$* {
        public static <fields>;
    }
     
    #避免混淆泛型 如果混淆报错建议关掉
    #–keepattributes Signature
     
    #移除log 测试了下没有用还是建议自己定义一个开关控制是否输出日志
    #-assumenosideeffects class android.util.Log {
    #    public static boolean isLoggable(java.lang.String, int);
    #    public static int v(...);
    #    public static int i(...);
    #    public static int w(...);
    #    public static int d(...);
    #    public static int e(...);
    #}
    
&nbsp;&nbsp;&nbsp;&nbsp;ok ,大概就这么一些了。