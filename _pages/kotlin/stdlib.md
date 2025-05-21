---
title: 常用函式庫
date: 2025-05-14
keywords: kotlin, stdlib
---
## 取出亂數
- [List][1]

{% highlight kotlin linenos %}
val randomId = (1 .. 100).shuffled().first()
{% endhighlight %}

shuffled()傳回List，first()取出List中的第一個元素。

[1]: {% link _pages/kotlin/list.md %}