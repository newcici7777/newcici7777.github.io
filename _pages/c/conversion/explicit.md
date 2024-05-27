---
title: 強制轉型
date: 2024-05-24
keywords: c++, explicit
---

將等號(=)右邊的值強制轉型，明確告訴編譯器等號(=)右邊的值的類型。

{% highlight c++ linenos %}
int a = (int)8.3;
{% endhighlight %}

另一種強制轉型的方式，將值放在括號中()。

{% highlight c++ linenos %}
int a = int(8.3);
{% endhighlight %}