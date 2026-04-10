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