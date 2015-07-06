---
layout: post
title: Material Design RecycleView(一）
categories: 技术
tags: RecycleView
---
> **本博客为个人原创，转载需在明显位置注明出处**
	
&nbsp;&nbsp;到今天才认真的看了下Material Desgin控件之一RecycleView,好吧，我是真懒。
	今天写了个demo算是初步了解下RecycleView,话不多说，直接上代码。
	
依赖哪些Library  

	{% highlight java linenos %}
	dependencies {
    	compile fileTree(dir: 'libs', include: ['*.jar'])
    	compile 'com.android.support:appcompat-v7:21.0.0'
    	compile 'com.android.support:recyclerview-v7:21.0.0'
	}
	{% endhighlight %}

layout布局文件

	{% highlight java linenos %}
	<android.support.v7.widget.RecyclerView
     	android:id="@+id/recyclerView"
     	android:layout_width="match_parent"
     	android:layout_height="match_parent"
     	android:overScrollMode="never"
     	android:fadingEage="none"
	/>
	{% endhighlight %}
         

然后在MainActivity.java         
    
    {% highlight java linenos %}
	mRecyclerView = (RecyclerView)findViewById(R.id.recyclerView);

    //设置默认的线性LayoutManager
    mRecyclerView.setLayoutManager(new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, false));

    //如果可以确定每个item的高度是固定的，设置这个选项可以提高性能
    mRecyclerView.setHasFixedSize(true);

    //创建并设置Adapter
    mRecyclerView.setAdapter(new BaseListAdapter(this));
    {% endhighlight %}
    
&nbsp;&nbsp;然后我们要设置Adapter，这是关键的一步。也和ListView有一点不一样。ViewHolder也不再是自定义的独立的类了，它们都要继承子RecyclerView的Adapter和ViewHolder内部类

	{% highlight java linenos %}
	public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder>{

    	public ArrayList<String> datas;

    	public MyAdapter(ArrayList<String> datas) {
        	this.datas = datas;
    	}

    //创建新View，被LayoutManager所调用
    	@Override
    	public ViewHolder onCreateViewHolder(ViewGroup viewGroup, int viewType) {
        View view = LayoutInflater.from(viewGroup.getContext()).inflate(R.layout.list_item,
                viewGroup, false);
        ViewHolder vh = new ViewHolder(view);
        return vh;
    }

    	//将数据与界面进行绑定的操作
   	 	@Override
    	public void onBindViewHolder(ViewHolder viewHolder, int position) {
        	viewHolder.mTextView.setText(datas.get(position));
    	}

    	//获取数据的数量
    	@Override
    	public int getItemCount() {
        	return datas.size();
    	}

    	//自定义的ViewHolder，持有每个Item的的所有界面元素
    	public static class ViewHolder extends RecyclerView.ViewHolder {
        	public TextView mTextView;

        	public ViewHolder(View view) {
            	super(view);
            	mTextView = (TextView) view.findViewById(R.id.text);
        	}
    	}
	}	
	{% endhighlight %}

&nbsp;&nbsp;看到了这个Adapter是不是觉得非常麻烦，我们可以查看RecyclerView源码，不难发现，RecyclerView.Adapter中已经帮助我们实现了convertView的优化复用，然后将实例化convertView和set value拆分成两个抽象方法(onCreateViewHolder()和onBindViewHolder())在子类来实现，
也就是onCreateViewHolder() + onBindViewHolder() = getView()；

这一节也就到这里了，简单介绍了RecycleView,当然RecycleView还有很多好用的特性。在后面的章节慢慢说，

    
    