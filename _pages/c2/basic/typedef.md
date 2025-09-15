---
title: typedef重新定義類型
date: 2025-09-12
keywords: c++, c,
---
# typedef重新定義類型

重新定義類型

重新定義類型int32\_t是int

```c
typedef int int32_t;
```

使用方法

{% highlight c++ linenos %}
int32_t k = 10;
printf("%d\n",sizeof(k));
{% endhighlight %}

執行結果

```
4
```

重新定義類型int64\_t為long long

```c
typedef long long int64_t;
```

使用方法

```c
int64_t m = 10;
printf("%d\n",sizeof(m));
```

