---
layout: post
title: android 中的 mvp (下)
categories: 技术
tags: mvp
---

   上一篇主要讲解了MVP的初步使用，现在我们来写一个实例来了解mvp
工程目录结构大概如下。

	![mvp](/images/mvpimage.png)

   model层提供抽象接口，方便解耦。
   
   Model层抽象接口
   {% highlight java linenos %}
   	/**
 	* Created by lenovo on 2015/6/15.
 	* 天气model的接口
 	*/
	public interface WeatherModel {
    	void loadWeather(OnWeatherListener listener);
	}
	{% endhighlight %}
   Model层抽象实现：
   
   	@Override
    public void loadWeather(final OnWeatherListener listener) {
        	String url = "http://www.weather.com.cn/adat/sk/101280601.html";
        	AsyncHttpClient client = new AsyncHttpClient();
        	client.get(url,new AsyncHttpResponseHandler(){
            @Override
            public void onSuccess(int i, Header[] headers, byte[] bytes){
                listener.Success(new String(bytes));
            }
            @Override
            public void onFailure(int i, Header[] headers, byte[] bytes, Throwable throwable) {
                listener.error();
            }
        });
    }
    
    
    
   View层的同样提供抽象接口，方便解耦。
   	
   	public interface WeatherView {
    	void showLoading();
    	void hideLoading();
    	void showError();
    	／／填充数据
    	void setWeatherInfo(String s);
	}

	
这样仅仅通过接口对接，再利用Presenter来负责对接UI与数据逻辑。
	 presenter接口类WeatherPresenter.java和OnWeatherListener.java
	 
	 /**
 	  * Created by techstan on 2015/6/15.
 	  * 获取天气的逻辑
 	  */
	 public interface WeatherPresenter {
   		void getWeather();
	 }
	 
-------------------------------------------
	 
	 /**
 	  * Created by techstan on 2015/6/15.
	  * 在Presenter层实现，给Model层回调，更改View层的状态，确保Model层不直       接操作View层
 	 */
 	 
	 public interface OnWeatherListener {
     /**
      * 成功的回调
      */
     void Success(String s);

     /**
     * 失败的回调
     */
     void error();
	
   Presenter的实现类  WeatherPresenterImpl.java

	/**
     * Created by techstan on 2015/6/15.
     * presenter的实现
     */
	public class WeatherPresenterImpl implements 		WeatherPresenter,OnWeatherListener{

    /*Presenter作为中间层，持有View和Model的引用*/
    private WeatherView weatherView;
    private WeatherModel weatherModel;

    public WeatherPresenterImpl(WeatherView weatherView){
        this.weatherView=weatherView;
        weatherModel=new WeatherModelImpl();
    }

    @Override
    public void getWeather() {
        weatherView.showLoading();
        weatherModel.loadWeather(this);
    }

    @Override
    public void Success(String s) {
        weatherView.hideLoading();
        weatherView.setWeatherInfo(s);
    }

    @Override
    public void error() {
        weatherView.hideLoading();
        weatherView.showError();
    }

   然后WeatherActivity中直接实现WeatherView接口，
   
  	@Override
	protected void initializeData() {
		 weatherPresenter = new WeatherPresenterImpl(this); //传入WeatherView
		 mDialog = new ProgressDialog(this);
		 mDialog.setTitle("加载天气中...");
		 weatherPresenter.getWeather();
	}

	@Override
	public void showLoading() {
		mDialog.show();
	}

	@Override
	public void hideLoading() {
		mDialog.dismiss();
	}

	@Override
	public void showError() {
		Toast.makeText(this, "error", 0).show();
	}

	@Override
	public void setWeatherInfo(String s) {
		try {
			JSONObject jsonObj = new JSONObject(s);
			tv_cityName.setText(jsonObj.getString("city"));
			tv_cityWD.setText(jsonObj.getString("wd"));
			tv_cityWS.setText(jsonObj.getString("ws"));
			tv_citySD.setText(jsonObj.getString("sd"));
		} catch (JSONException e) {
			e.printStackTrace();
		}
	}

这样一个mvp架构的实例就出来了。从实例中可以看出，view中只负责处理ui部分，Presenter调用Model处理完数据之后，再通过View的抽象接口更新View显示的信息，实现了完整的解耦UI与逻辑操作，可能小型app这样写感觉比较麻烦，但是一旦项目做大，那么很有必要采用这种mvp架构，这样分工更明确，代码结构层次更清晰。也更方便调试

