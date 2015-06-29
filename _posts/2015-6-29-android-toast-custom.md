---
layout: post
title: Android自定义Toast
categories: 技术
tags: custom Toast 
---
> **本博客为个人原创，转载需在明显位置注明出处**

&nbsp;&nbsp;这应该算是很早以前一个小技巧了，当我们多次弹出toast的时候，toast会依此得显示，我们并不希望如此，我们希望不要一直重复的提醒了。
	
&nbsp;&nbsp;直接贴代码吧！
	{% highlight java linenos %}
	public class CustomToast{
	
		private static Toast mToast;
		private static LayoutInflater inflater;
		private static Handler mHandler = new Handler();
		private static Runnable r = new Runnable() {
		public void run() {
			mToast.cancel();
			}
		};
	
	
		public static Toast makeText(Context context,CharSequence text, int duration){
			mToast = new Toast(context);
			inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE); 
			//自定义ToastUI  
			View layout = inflater.inflate(R.layout.caff_toast_utils, null);  
        	TextView textView = (TextView) layout.findViewById(R.id.toast_text);  
	    	textView.setText(text);  
	    	mToast.setView(layout);  
	    	mToast.setGravity(Gravity.CENTER_VERTICAL, 0, 0);  
	    	mToast.setDuration(duration);  
			return mToast;
		}

		public static void showToast(Context mContext, String text, int duration) {
			//关键代码
			mHandler.removeCallbacks(r);
			if (mToast != null){
				inflater = (LayoutInflater) mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);   
				View layout = inflater.inflate(R.layout.caff_toast_utils, null);  
	        	TextView textView = (TextView) layout.findViewById(R.id.toast_text);  
		    	textView.setText(text);  
		    	mToast.setView(layout);
			}
			else{
				mToast = makeText(mContext, text, duration);
			}
			
			mHandler.postDelayed(r, 2000);
			mToast.show();
		}
	
		/**
	 	* 可以直接使用id
	 	* method desc：
	 	* @param mContext
	 	* @param resId
		 * @param duration
	 	*/
		public static void showToast(Context mContext, int resId, int duration) {
			showToast(mContext, mContext.getResources().getString(resId), duration);
		}
	}
	{% endhighlight %}

&nbsp;&nbsp;代码非常简单。通常我们在开发过程中，使用最多的是makeText来显示toast.但这样也会有一个问题，因为每次都new一个toast出来，造成应用退出后都不能取消toast。而通过自定义，开启线程取消toast也更好的解决了这个问题。这也算是开发中的一个小技巧。