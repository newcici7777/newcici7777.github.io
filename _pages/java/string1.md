---
title: String
date: 2025-05-26
keywords: Java, String
---
## 字串常數
String用來儲存一段文字，或稱為字串常數。

什麼是字串常數？\"\"雙引號包住的字串是常數。

{% highlight java linenos %}
String str = "這個就是字串常數";
{% endhighlight %}

## final char陣列
String其實是final不可被繼承的char\[\]字元陣列。

## 建構子
{% highlight java linenos %}
String s1 = new String();
String s2 = new String(String s);
{% endhighlight %}

./build/macosx-x86_64-server-fastdebug/jdk/bin/java -XX:+UnlockDiagnosticVMOptions -XX:+PrintVtableStats -version