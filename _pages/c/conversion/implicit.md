---
title: 自動轉型
date: 2024-05-24
keywords: c++, implicit
---

編譯器根據等號(=)左邊的變數資料型態，決定調用那個轉型函式。

{% highlight c++ linenos %}
int a = 8.3;
{% endhighlight %}

等號(=)左邊的變數資料型態是int，等號(=)右邊的值是double，編譯器根據等號(=)左邊的變數資料型態int，把等號(=)右邊的資料型態double轉成int。
