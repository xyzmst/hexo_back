title: webveiw详解
date: 2016-06-20 18:04:26
tags:
---
### 写在前面
这个算是 知识点的整理，或者叫笔记吧，参考链接在最下面，也有使用时的注意事项之类的
### 设置
- js支持 
 ```
 WebView.getSettings().setJavaScriptEnabled(true);
 ```
- 禁止打开外部浏览器
```
mWebView.setWebViewClient(new WebViewClient(){
	public boolean shouldOverrideUrlLoading(WebView view, String url){ 
		view.loadUrl(url);
		return true;
	}
});
```
- 返回键页面历史回退
```
public boolean onKeyDown(int keyCode, KeyEvent event) {
        if ((keyCode == KeyEvent.KEYCODE_BACK) && mWebView.canGoBack()) {
            mWebView.goBack();
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }
```
### 与js互调
- 
注:android 4.2 之后版本提供给js调用的函数必须带有注释语句@JavascriptInterface
否则回报 "Uncaught TypeError: window.demo.clickOnAndroid is not a function
```
mWebView.addJavascriptInterface(new Object() {
    @JavascriptInterface
	  public void clickOnAndroid() {
		  mHandler.post(new Runnable() {
			  public void run() { 
				  mWebView.loadUrl("javascript:wave()");
			  }
		  });
	  }
	}, "demo"); 
	mWebView.loadUrl("file:///android_asset/demo.html"); 
```
html 页面
```
<html>
<script language="javascript">
  function wave() {
    document.getElementById("droid").src="android_waving.png";
  }
</script>
<body>
  <a onClick="window.demo.clickOnAndroid()">
  <img id="droid" src="android_normal.png" mce_src="android_normal.png"/><br> Click me! </a>
</body>
</html>
```
### 深入使用WebView
- WebView、WebViewClient、WebChromeClient 之间的区别:
在 WebView 的设计中,不是什么事都要 WebView类干的,有些杂事是分给其他人的,
这样 WebView 专心干好 自己的解析、渲染工作就行了.WebViewClient 就是帮助 
WebView 处理各种通知、请求事件等 ,WebChromeClient 是辅助 WebView 
处理 Javascript 的对话框,网站图标,网站 title.
### 缓存
```
//优先使用缓存: 
WebView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK); 
//不使用缓存: 
WebView.getSettings().setCacheMode(WebSettings.LOAD_NO_CACHE);
```

- 在退出应用的时候加上如下代码，可以完整的清空缓存
```
	File file = CacheManager.getCacheFileBaseDir();
	if (file != null && file.exists() && file.isDirectory()) {
		for (File item : file.listFiles()) {
			item.delete();
		}
		file.delete();
	}
	context.deleteDatabase("WebView.db"); 
	context.deleteDatabase("WebViewCache.db");
```
删除保存于手机上的缓存
```
private int clearCacheFolder(File dir,long numDays) { 
  int deletedFiles = 0;
  if (dir!= null && dir.isDirectory()){
    try {
      for (File child:dir.listFiles()){
	      if (child.isDirectory()) {
	        deletedFiles += clearCacheFolder(child, numDays);
        }
        if (child.lastModified() < numDays) {
          if (child.delete()) {
           deletedFiles++; 
          }
        }
      }
    } catch(Exception e) {
      e.printStackTrace(); 
    }
  }
  return deletedFiles; 
}
```
- 404 处理
```
public class WebView_404 extends Activity {	
	private Handler handler = new Handler() {
    public void handleMessage(Message msg) {
      if(msg.what==404) {//主页不存在
        //载入本地 assets 文件夹下面的错误提示页面 404.html 
        web.loadUrl("file:///android_asset/404.html");
      }else{
        web.loadUrl(HOMEPAGE);
      }
    }
	};
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	  web.setWebViewClient(new WebViewClient() {
  	public boolean shouldOverrideUrl(WebView view,String url) { 
	    if(url.startsWith("http://") && getRespStatus(url)==404) {
	      view.stopLoading();
	      //载入本地 assets 文件夹下面的错误提示页面 404.html 
	      view.loadUrl("file:///android_asset/404.html");
	    }else{
	      view.loadUrl(url);
	    }
	      return true;
	    }
	  });
	  new Thread(new Runnable() {
    public void run() {
		  Message msg = new Message();
		  //此处判断主页是否存在,因为主页是通过 loadUrl 加载的,
		  //此时不会执行 shouldOverrideUrlLoading 进行页面是否存在的判断 //进入主页后,点主页里面的链接,链接到其他页面就一定会执行
		  shouldOverrideUrlLoading 方法了 
		  if(getRespStatus(HOMEPAGE)==404) {
	      msg.what = 404;
		  }
		  handler.sendMessage(msg);
	  }).start();
  }
}
```
### 判断 WebView 是否已经滚动到页面底端
- 启动或关闭webview的缩放功能。
```
mWebView.getSettings().setSupportZoom(true);
mWebView.getSettings().setBuiltInZoomControls(true);
```
- 如果已经处于底端
```
if(WebView.getContentHeight*WebView.getScale() -(webvi ew.getHeight()+WebView.getScrollY())){ 
  //XXX
}
```

### WebView获取服务器中的 session 问题

- Android 中的 WebView 如何获取服务器页面的 jsessionid 的值
- Android 的 WebView 又是如何把得到的 jsessionid 的值在 set 到服务器中,一致达到他们在同一个 jsessionid 的回话中.
```
CookieManager cm = CookieManager.getInstance(); 
cm.removeAllCookie();
cm.getCookie(url);
cm.setCookie(url, cookie);
```
工具方法
```
    /***
     * 如果用户已经登录，则同步本地的cookie到webview中
     */
    public void synCookies() {
        if (!CacheUtils.isLogin(this)) return;
        CookieSyncManager.createInstance(this);
        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.setAcceptCookie(true);
        cookieManager.removeSessionCookie();//移除
        String cookies = PreferenceHelper.readString(this, AppConfig.COOKIE_KEY, AppConfig.COOKIE_KEY);
        KJLoger.debug(cookies);
        cookieManager.setCookie(url, cookies);
        CookieSyncManager.getInstance().sync();
    }
```



### 参考链接
- [深入讲解webview](http://www.kymjs.com/code/2015/05/03/01)
- [深入讲解WebView——下](http://kymjs.com/code/2015/05/04/01/)
