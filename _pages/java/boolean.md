---
title: boolean
date: 2025-06-25
keywords: Java, boolean
---
## 語法
boolean只能設true與false。
{% highlight java linenos %}
boolean flag = true;
boolean flag = false;
{% endhighlight %}

不可以用0或非0的整數指派給boolean，C++可以，Java不行。

以下是錯誤寫法。
{% highlight java linenos %}
boolean flag = 1;
boolean flag = 0;
boolean flag = null;
{% endhighlight %}

## 無法轉型基本數字型態
以下是錯誤的語法，無法把boolean轉成int。
{% highlight java linenos %}
  boolean flag = true;
  int i = flag;
{% endhighlight %}

## boolean與String互相轉型
跟整數、浮點數一樣，可以跟String互相轉型。

{% highlight java linenos %}
boolean flag = true;
String ss = flag + "";
System.out.println(ss);

boolean flag2 = Boolean.parseBoolean("true");
System.out.println(flag2);
{% endhighlight %}
```
true
true
```

