---
title: dio
date: 2026-04-10
keywords: flutter, dio
---
打開終端機，在專案目錄下，輸入以下內容。<br>
```
flutter_base % flutter pub add dio
```

確認有dio版本。<br>
![img]({{site.imgurl}}/flutter/dio1.png)<br>

語法
```
Dio().get(網址).then((res) {
    成功
  }).catchError((error) {
    失敗
  });
```

重新run。<br>
{% highlight dart linenos %}
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';

void main() {
  Dio().get('https://www.httpbin.org/get').then((res) {
    print(res.data); // ""
  }).catchError((error) {
    print(error);
  });
}
{% endhighlight %}
```
This app is linked to the debug service: ws://127.0.0.1:49201/iD0_5D0mbiE=/ws
Debug service listening on ws://127.0.0.1:49201/iD0_5D0mbiE=/ws
Connecting to VM Service at ws://127.0.0.1:49201/iD0_5D0mbiE=/ws
{args: {}, headers: {Accept: */*, Accept-Encoding: gzip, deflate, br, zstd, Accept-Language: zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7, Host: www.httpbin.org, Origin: http://localhost:65521, Priority: u=1, i, Referer: http://localhost:65521/, Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146", Sec-Ch-Ua-Mobile: ?0, Sec-Ch-Ua-Platform: "macOS", Sec-Fetch-Dest: empty, Sec-Fetch-Mode: cors, Sec-Fetch-Site: cross-site, User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36, X-Amzn-Trace-Id: Root=1-69d89760-0e88bc5a1d8c9f993cf902c8}, origin: 42.73.190.130, url: https://www.httpbin.org/get}
```

## Dio 封裝
{% highlight dart linenos %}
class DioUtils {
  final Dio _dio = Dio();
  DioUtils(){
    _dio.options.baseUrl = 'https://www.httpbin.org';
    _dio.options.connectTimeout = Duration(seconds: 10);  // 連線超出時間
    _dio.options.sendTimeout = Duration(seconds: 10);     // 傳送超出時間
    _dio.options.receiveTimeout = Duration(seconds: 10);  // 接收超出時間
  }
}
{% endhighlight %}

### 使用.. 鏈式呼叫
鏈式呼叫(chaining call)，可以在一個執行語句上，呼叫多次方法或指派多次屬性。<br>
{% highlight dart linenos %}
class DioUtils {
  final Dio _dio = Dio();
  DioUtils() {
    _dio.options
      ..baseUrl = 'https://www.httpbin.org'
      ..connectTimeout = Duration(seconds: 10)
      ..sendTimeout = Duration(seconds: 10)
      ..receiveTimeout = Duration(seconds: 10);
  }
}
{% endhighlight %}

### 增加攔截器
Interceptors是抽象類別，無法建立物件，要由子類別InterceptorsWrapper，才能建立物件。<br>
{% highlight dart linenos %}
  void _addInterceptors() {
    _dio.interceptors.add(InterceptorsWrapper(
      // 請求
      onRequest: (context, handler) {
        return handler.next(context);
      },
      // 伺服器回應
      onResponse: (context, handler) {
      	// statusCode 介於200 ~ 299
        if (context.statusCode! >= 200 && context.statusCode! < 300) {
          return handler.next(context);
        }
        // statusCode 不是介於200 ~ 299 丟出Exception
        handler.reject(DioException(requestOptions: context.requestOptions));
      },
      // 處理Exception
      onError: (error, handler) {
      	// 丟出Exception
        return handler.reject(error);
      },
    ));
  }

{% endhighlight %}

### 處理get 請求
傳回值類型是Future。
{% highlight dart linenos %}
  Future<Response<dynamic>> get(String url,
      {Map<String, dynamic>? queryParameters}) async {
    return _dio.get(url, queryParameters: queryParameters);
  }
{% endhighlight %}

### 完整程式碼
{% highlight dart linenos %}
class DioUtils {
  final Dio _dio = Dio();
  DioUtils() {
    _dio.options
      ..baseUrl = 'https://www.httpbin.org'
      ..connectTimeout = Duration(seconds: 10)
      ..sendTimeout = Duration(seconds: 10)
      ..receiveTimeout = Duration(seconds: 10);
  }
  void _addInterceptors() {
    _dio.interceptors.add(InterceptorsWrapper(
      onRequest: (context, handler) {
        return handler.next(context);
      },
      onResponse: (context, handler) {
        if (context.statusCode! >= 200 && context.statusCode! < 300) {
          return handler.next(context);
        }
        handler.reject(DioException(requestOptions: context.requestOptions));
      },
      onError: (error, handler) {
        return handler.reject(error);
      },
    ));
  }

  Future<Response<dynamic>> get(String url,
      {Map<String, dynamic>? queryParameters}) async {
    return _dio.get(url, queryParameters: queryParameters);
  }
}

{% endhighlight %}

## Dio 與 async、Future
async 接收傳回值語法:
```
傳回值類型 函式名() async {
  類型 result = await Future(
    () {
      return 傳回值;
    },
  );
  等待上一步執行成功後會輸出result
}
```

Dio get()方法傳回值類型是Future。
{% highlight dart linenos %}
void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response =  await dioUtils.get('/get', queryParameters: {'name': 'cici'});
  print(response);
}
{% endhighlight %}

完整程式碼
{% highlight dart linenos %}
void main() {
  test();
}

void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response =  await dioUtils.get('/get', queryParameters: {'name': 'cici'});
  print(response);
}

class DioUtils {
  final Dio _dio = Dio();
  DioUtils() {
    _dio.options
      ..baseUrl = 'https://www.httpbin.org'
      ..connectTimeout = Duration(seconds: 10)
      ..sendTimeout = Duration(seconds: 10)
      ..receiveTimeout = Duration(seconds: 10);
  }
  void _addInterceptors() {
    _dio.interceptors.add(InterceptorsWrapper(
      onRequest: (context, handler) {
        return handler.next(context);
      },
      onResponse: (context, handler) {
        if (context.statusCode! >= 200 && context.statusCode! < 300) {
          return handler.next(context);
        }
        handler.reject(DioException(requestOptions: context.requestOptions));
      },
      onError: (error, handler) {
        return handler.reject(error);
      },
    ));
  }

  Future<Response<dynamic>> get(String url,
      {Map<String, dynamic>? queryParameters}) async {
    return _dio.get(url, queryParameters: queryParameters);
  }
}
{% endhighlight %}
```
{"args":{"name":"cici"},"headers":{"Accept":"*/*","Accept-Encoding":"gzip, deflate, br, zstd","Accept-Language":"zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7","Host":"www.httpbin.org","Origin":"http://localhost:49906","Priority":"u=1, i","Referer":"http://localhost:49906/","Sec-Ch-Ua":"\"Google Chrome\";v=\"147\", \"Not.A/Brand\";v=\"8\", \"Chromium\";v=\"147\"","Sec-Ch-Ua-Mobile":"?0","Sec-Ch-Ua-Platform":"\"macOS\"","Sec-Fetch-Dest":"empty","Sec-Fetch-Mode":"cors","Sec-Fetch-Site":"cross-site","User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/147.0.0.0 Safari/537.36","X-Amzn-Trace-Id":"Root=1-69dc7ff2-515f14f57c4ff2de0b0047a5"},"origin":"42.73.190.130","url":"https://www.httpbin.org/get?name=cici"}
```

## Response 泛型
Response的data成員變數為dynamic 類型
```
Response<dynamic> response = await dioUtils.get('/get');
```

Response 原始檔
{% highlight dart linenos %}
class Response<T> {
  // T類型根據使用者自訂，若T為dynamic，data就是dynamic類型
  T? data;
{% endhighlight %}

## Response 成員變數
{% highlight dart linenos %}
response.data        // 真正的資料（最重要）
response.statusCode  // 狀態碼（200、404）
response.headers     // 標頭
response.requestOptions // 請求資訊
{% endhighlight %}

## Response.data 取出資料
前面提過，data都是dynamic類型，如果要使用，必須轉型。<br>
dynamic是父類別，若要把父類別dynamic轉成子類別` Map<String, dynamic>`，要使用as。<br>

將response.data轉成Map類型。
```
Map<String, dynamic> res = response.data as Map<String, dynamic>;
```

轉成Map類型後，才可以使用Map方括號`[key]`取出資料<br>
```
res["args"];
```

{% highlight dart linenos %}
void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response =
      await dioUtils.get('/get', queryParameters: {'name': 'cici'});
  Map<String, dynamic> res = response.data as Map<String, dynamic>;
  print(res["args"]);
}
{% endhighlight %}
```
{name: cici}
```

## cast 轉型
假設今天的response.data的資料如下。
```
{data: {channels: [{id: 0, name: 推荐}, {id: 1, name: html}, {id: 2, name: 开发者资讯}, {id: 4, name: c++}, {id: 6, name: css}, {id: 7, name: 数据库}, {id: 8, name: 区块链}, {id: 9, name: go}, {id: 10, name: 产品}, {id: 11, name: 后端}, {id: 12, name: linux}, {id: 13, name: 人工智能}, {id: 14, name: php}, {id: 15, name: javascript}, {id: 16, name: 架构}, {id: 17, name: 前端}, {id: 18, name: python}, {id: 19, name: java}, {id: 20, name: 算法}, {id: 21, name: 面试}, {id: 22, name: 科技动态}, {id: 23, name: js}, {id: 24, name: 设计}, {id: 25, name: 数码产品}, {id: 26, name: 软件测试}]}, 
message: OK}
```

response.data是Map資料結構，有二個key，分別是:
```
data
message
```

response.data["data"]裡面是Map資料結構，有一個key是channels。
{% highlight dart linenos %}
void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response = await dioUtils.get('channels');
  Map<String, dynamic> res = response.data as Map<String, dynamic>;
  print(res["data"]);
}
{% endhighlight %}
```
{channels: [{id: 0, name: 推荐}, {id: 1, name: html}, {id: 2, name: 开发者资讯}, {id: 4, name: c++}, {id: 6, name: css}, {id: 7, name: 数据库}, {id: 8, name: 区块链}, {id: 9, name: go}, {id: 10, name: 产品}, {id: 11, name: 后端}, {id: 12, name: linux}, {id: 13, name: 人工智能}, {id: 14, name: php}, {id: 15, name: javascript}, {id: 16, name: 架构}, {id: 17, name: 前端}, {id: 18, name: python}, {id: 19, name: java}, {id: 20, name: 算法}, {id: 21, name: 面试}, {id: 22, name: 科技动态}, {id: 23, name: js}, {id: 24, name: 设计}, {id: 25, name: 数码产品}, {id: 26, name: 软件测试}]}
```

而response.data["data"]["channels"]裡面的資料則是list
{% highlight dart linenos %}
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response = await dioUtils.get('channels');
  Map<String, dynamic> res = response.data as Map<String, dynamic>;
  print(res["data"]["channels"]);
{% endhighlight %}
```
[{id: 0, name: 推荐}, {id: 1, name: html}, {id: 2, name: 开发者资讯}, {id: 4, name: c++}, {id: 6, name: css}, {id: 7, name: 数据库}, {id: 8, name: 区块链}, {id: 9, name: go}, {id: 10, name: 产品}, {id: 11, name: 后端}, {id: 12, name: linux}, {id: 13, name: 人工智能}, {id: 14, name: php}, {id: 15, name: javascript}, {id: 16, name: 架构}, {id: 17, name: 前端}, {id: 18, name: python}, {id: 19, name: java}, {id: 20, name: 算法}, {id: 21, name: 面试}, {id: 22, name: 科技动态}, {id: 23, name: js}, {id: 24, name: 设计}, {id: 25, name: 数码产品}, {id: 26, name: 软件测试}]
```

把response.data["data"]["channels"] 轉型成 List
{% highlight dart linenos %}
void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response = await dioUtils.get('channels');
  Map<String, dynamic> res = response.data as Map<String, dynamic>;
  // 轉型成List
  List list = res["data"]["channels"] as List<dynamic> ;
}
{% endhighlight %}

而每個元素的資料結構是`Map<String, dynamic>`，下方key為id，value的類型為int，key為name，value的類型為String。
```
{id: 0, name: 推荐}
```

把List中每個元素轉型成`Map<String, dynamic>`，因為是轉型list中的每個元素，使用cast。
{% highlight dart linenos %}
void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response = await dioUtils.get('channels');
  Map<String, dynamic> res = response.data as Map<String, dynamic>;
  // 轉成list 但裡面的元素仍為dynamic
  List list = res["data"]["channels"] as List<dynamic>;
  // 裡面的元素轉成Map<String, dynamic> 類型
  List<Map<String, dynamic>> _list = list.cast<Map<String, dynamic>>();
}
{% endhighlight %}

`List<dynamic>` 不能直接變成 `List<Map<String,dynamic>>`，必須用cast。<br>

其它替代寫法:把每一個元素dynamic轉成`Map<String, dynamic>`
{% highlight dart linenos %}
_list = (res["data"]["channels"] as List)
    .map((e) => e as Map<String, dynamic>)
    .toList();
{% endhighlight %}

完整程式碼
{% highlight dart linenos %}
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';

void main() {
  test();
}

void test() async {
  DioUtils dioUtils = DioUtils();
  Response<dynamic> response = await dioUtils.get('channels');
  Map<String, dynamic> res = response.data as Map<String, dynamic>;
  // 轉成list 但裡面的元素仍為dynamic
  List list = res["data"]["channels"] as List<dynamic>;
  // 轉成list，裡面的元素也轉成Map<String, dynamic> 類型
  List<Map<String, dynamic>> _list = list.cast<Map<String, dynamic>>();
}

class DioUtils {
  final Dio _dio = Dio();
  DioUtils() {
    _dio.options
      ..baseUrl = 'https://geek.itheima.net/v1_0/'
      //..baseUrl = 'https://www.httpbin.org'
      ..connectTimeout = Duration(seconds: 10)
      ..sendTimeout = Duration(seconds: 10)
      ..receiveTimeout = Duration(seconds: 10);
  }
  void _addInterceptors() {
    _dio.interceptors.add(InterceptorsWrapper(
      onRequest: (context, handler) {
        return handler.next(context);
      },
      onResponse: (context, handler) {
        if (context.statusCode! >= 200 && context.statusCode! < 300) {
          return handler.next(context);
        }
        handler.reject(DioException(requestOptions: context.requestOptions));
      },
      onError: (error, handler) {
        return handler.reject(error);
      },
    ));
  }

  Future<Response<dynamic>> get(String url,
      {Map<String, dynamic>? queryParameters}) async {
    return _dio.get(url, queryParameters: queryParameters);
  }
}
{% endhighlight %}