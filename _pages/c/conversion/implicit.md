---
title: 隱式轉型
date: 2024-05-24
keywords: c++, implicit
---

編譯器根據等號(=)左邊的變數類型，決定調用那個轉換函式。

{% highlight c++ linenos %}
int a = 8.3;
{% endhighlight %}

等號(=)左邊的變數類型是int，等號(=)右邊的值是double，編譯器根據等號(=)左邊的變數類型int，把等號(=)右邊的類型double轉成int。
