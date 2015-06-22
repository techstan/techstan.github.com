---
layout: post
title: 如何更好的退出Android APP
categories: 技术
tags: exit app
---
> **本博客为个人原创，转载需在明显位置注明出处**

&emsp;&emsp;在以往的开发过程中，我们通常是利用一个自定义的AppManager，将所有的Activity添加到这个栈里面。退出时逐个finish.

&emsp;&emsp;其实本身的Activity有自己的ActivityManager系统来帮我们管理这个栈。如果自己写一套，可能会造成activity不能释放，不稳当的因素。

&emsp;&emsp;google官方文档中有讲解到给activity添加launchMode
https://developer.android.com/guide/components/tasks-and-back-stack.html

&emsp;&emsp;关于四种启动，standard,singleTop,singleTask,singleInstance这里不再赘述。

&emsp;&emsp;为了更好的退出app，我们将利用到四种启动模式的singleTask.

&emsp;&emsp;The system creates a new task and instantiates the activity at the root of the new task. However, if an instance of the activity already exists in a separate task, the system routes the intent to the existing instance through a call to its onNewIntent() method, rather than creating a new instance. Only one instance of the activity can exist at a time.

	
&emsp;&emsp;大概意思是，以"singleTask"方式启动的Activity，全局只有唯一个实例存在，因此，当我们第一次启动这个Activity时，系统便会创建一个新的任务，并且初始化一个这样的Activity的实例，放在新任务的底部，如果下次再启动这个Activity时，系统发现已经存在这样的Activity实例，就会调用这个Activity实例的onNewIntent成员函数，从而把它激活起来。从这句话就可以推断出，以"singleTask"方式启动的Activity总是属于一个任务的根Activity。

&emsp;&emsp;利用将首页MainActivity的launchMode配置成singleTask( 当然也可以配置为singleTop)

&emsp;&emsp;然后在MainActivity 的onNewInstance(Intent intent) 

	{% highlight java linenos %}
	@Override
	protected void onNewIntent(Intent intent) {
		super.onNewIntent(intent);
		if(intent.getBooleanExtra("exitApp", false)){
			finish();
		}
	}
	{% endhighlight %}

&emsp;&emsp;这样当我们在任意的activity中只要执行下面这段代码就可以达到退出app的效果，而不是使用System.exit(0),

	{% highlight java linenos %}
	Intent intent=new Intent(this,MainActivity.class);
	intent.putExtra("exitApp", true);
	startActivity(intent);
	//跳转到MainActivity中执行onNewInstance(intent)
	{% endhighlight %} 
	
&emsp;&emsp;简短的几句代码，但是更能高效，安全的处理app退出这个常用的操作。当然了不仅仅是singleTask,我们也可以利用singleTop配合Intent.FLAG_ACTIVITY_CLEAR_TOP来实现同样的效果。 