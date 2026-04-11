---
title: const final 常數
date: 2026-04-10
keywords: flutter, const,final
---
常數、變數使用時機:<br>
- 存儲變化的資料用變數
- 存儲不變的資料，編譯時期就確認有值，用const 常數。
- 存儲不變的資料，編譯時期未初始化值，執行時期才會確定有值或產生物件，用final。

## const
語法
```
const 常數名 = 值;
```
const 是編譯時就要指派值，且指派後不可更改，以下編譯錯誤。<br>
{% highlight dart linenos %}
  const PI = 3.1415926;
  PI = 222;
{% endhighlight %}

## final
語法
```
final 常數名 = 值 / 類別建構子/ 函式的回傳值;
final x = 1234;  // 值
final Dio dio = Dio();  // 建構子
final time = DateTime.now();  // 函式的回傳值
```

final 是編譯的時候可以沒有值，但執行的時候一定要有初始值。<br>
以下的程式碼，編譯的時候`DateTime.now()`還未執行，還未初始化，執行後才會有值。<br>
{% highlight dart linenos %}
  final time = DateTime.now();
  print(time);
{% endhighlight %}

以下的語法，Dio()建構子在編譯時期，不能呼叫，執行時期才會建立物件，指派給dio變數。<br>
{% highlight dart linenos %}
  final Dio dio = Dio();
{% endhighlight %}

final 也是指派值後不可更改，即便那個值在編譯時期是還沒產生出新的值。<br>
以下編譯錯誤。<br>
{% highlight dart linenos %}
  final time = DateTime.now();
  time = DateTime.now();
  print(time);
{% endhighlight %}

