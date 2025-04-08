---
title: Laravel基本介紹
date: 2023-05-17
keywords: Laravel
---
首先，使用 Composer 下載 Laravel installer：  
```
composer global require "laravel/installer"
```

請確定把 $HOME/.composer/vendor/bin 路徑放置於環境變數 $PATH 裡，這樣你的系統才能找到 laravel 執行檔。  
一旦安裝完成後，就可以使用 laravel new 指令在指定的目錄建立一份全新安裝的 Laravel。例如：laravel   new blog 將會建立一個名稱為 blog 的目錄，裡面存放著全新安裝的 Laravel 和相依程式碼：  
```
laravel new blog
```

名詞介紹:  
- routes路徑表:網址路由，指定某個網址要由哪個 controller 來負責處理。
- controllers控制器:MVC 中的 controller 的程式碼檔案，你的程式碼會在這裡處理各種資料後丟給 View (HTML 網頁模版)來顯示。
- views畫面:MVC 中的 view ， HTML 網頁模版。

目錄介紹:  
- Public 目錄
	安裝完 Laravel 之後，需要將您的網站伺服器根目錄指向 public 目錄，HTTP 請求的進入點。
- Resource目錄
	放置JS原始檔案/Image/font ...等等。
	放置HTML網頁模版的View目錄
- Http/Controllers目錄
	放置Controller程式
- Routes
	放置所有網址路由檔案(api.php/web.php)

基本 GET 路由
{% highlight php linenos %}
Route::get('/', function()
{
    return 'Hello World';
});
{% endhighlight %}

基本 POST 路由
{% highlight php linenos %}
Route::post('foo/bar', function()
{
    return 'Hello World';
});
{% endhighlight %}

Controller控制器
{% highlight php linenos %}
	class TestController extends Controller
	{
	    public function showUser($id,$name)
	    {
	        echo 'UserId:'.$id.'/UserName:'.$name;
	    }
	    public function Index() {
	        return view('User.userinfo',['name' => 'CiCi']);
	    }
	}
{% endhighlight %}

Route路由
{% highlight php linenos %}
	Route::get('showUser/{id}/{name}', [TestController::class, 'showUser']);
	Route::get('showUser', [TestController::class, 'index']);
{% endhighlight %}

webpack.mix.js  
JS檔案放置在resouces/js目錄下，透過webpack.mix.js會把js檔案打包並放置在public/js目錄下。  
```
mix.js('resources/js/User/user.js', 'public/js/User');
```

執行以下指令  
1. composer update
2. php artisan key:generate
3. npm install
4. npm run dev

會打包resouces/js到public/js  
打包成功頁面  
![img]({{site.imgurl}}/laravel/laravel1.png)
![img]({{site.imgurl}}/laravel/laravel2.png)
![img]({{site.imgurl}}/laravel/laravel3.png)

Vue.js
範例1:
https://jsbin.com/tujicecano/edit?html,js,output  
範例2:
https://jsbin.com/vesucohani/edit?html,js,output  
date convert to utc+8
https://jsbin.com/darebapafe/edit?html,js,output  

Laravel若無法執行，可先執行這行
```
export PATH="$PATH:$HOME/.composer/vendor/bin"
vim ~/.zshrc
. ~/.zshrc
```

npm相關
```
ADD  "webpack": "^5.23.0"  in package.json
npm uninstall sass
npm uninstall sass-loader
rm -rf node_modules
rm package-lock.json
npm cache clear --force
npm install
```

[.env安裝](https://www.evernote.com/shard/s548/client/snv?noteGuid=67d6e610-96dd-6adc-4510-14e019926bcc&noteKey=5c0cc7b5f0bb03f87c595c79f190d6f0&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs548%2Fsh%2F67d6e610-96dd-6adc-4510-14e019926bcc%2F5c0cc7b5f0bb03f87c595c79f190d6f0&title=ec-console%2B%25E7%2592%25B0%25E5%25A2%2583%25E6%259E%25B6%25E8%25A8%25AD)