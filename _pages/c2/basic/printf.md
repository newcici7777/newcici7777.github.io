---
title: 格式化占位符
date: 2025-09-12
keywords: c++, c, printf
---

# 印出格式化占位符

## 整型

unsinged代表只有正數，沒有unsinged代表有正負數

| 整型             | byte | 占位  |
| -------------- | ---- | --- |
| int            | 4    | %d  |
| unsinged int   | 4    | %u  |
| short          | 2    | %hd |
| unsinged short | 2    | %hu |
| long           | 4    | %ld |
| unsinged long  | 4    | %ud |
| char           | 1    | %c  |
| char           | 1    | %d  |

注意，char也屬於整型，也可使用%d印出，以上表格列出的整型都可以使用%d印出，只是印出的精準度不同。

{% highlight c++ linenos %}
char c = 'a';
printf("%d\n", c);
printf("%c\n", c);
{% endhighlight %}

執行結果

```
97
a
```

sizeof

可以使用sizeof()查看所占的byte

{% highlight c++ linenos %}
    //類型
    int i = 10;
    long j = 10;
    long long l = 0;
    printf("%d\n",sizeof(i));
    printf("%d\n",sizeof(j));
    printf("%d\n",sizeof(l));
{% endhighlight %}

由於環境是mac，long印出的byte數為8

執行結果

```
4
8
8
```

浮點數

| 浮點數         | byte | 格式化 |
| ----------- | ---- | --- |
| float       | 4    | %f  |
| double      | 8    | %lf |
| long double | 8    | %Lf |

印出Float

第2行，%f，預設印出小數點後6位

第3行，%.2f，只印出小數點後2位

{% highlight c++ linenos %}
    float f1 = 3.8;
    printf("f1 = %f \n", f1);
    printf("f1 = %.2f \n", f1);
{% endhighlight %}

執行結果

```
f1 = 3.800000
f1 = 3.80
```

印出double

double跟float都可以%f印出，也可用%lf，但float只能用%f

{% highlight c++ linenos %}
    float f1 = 3.8;
    double d1 = 3.8;
    printf("f1 = %f , d1 = %f , d1 = %lf\n", f1, d1, d1);
    printf("f1 = %.2f , d1 = %.2f , d1 = %.2lf\n", f1, d1, d1);
{% endhighlight %}

執行結果

<pre><code><strong>f1 = 3.800000 , d1 = 3.800000 , d1 = 3.800000
</strong>f1 = 3.80 , d1 = 3.80 , d1 = 3.80
</code></pre>

