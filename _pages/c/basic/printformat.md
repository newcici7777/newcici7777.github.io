---
title: printf格式化輸出
date: 2024-04-18
keywords: c++, printf
---

# 資料型態 

## 整數
### 有正負整數資料型態

|整數型態|byte|格式符|
|:---:|:---:|:---:|
|int  |4    |%d   |
|short|2    |%hd  |
|long |4    |%ld  |


### Unsigned正整數資料型態
Unsinged代表只有正數。

|整數型態       |byte |格式符|
|:---:         |:---:|:---:|
|unsinged int  |4    |%u   |
|unsinged short|2    |%hu  |
|unsinged long |4    |%ud  |

### char資料型態

|整數型態|byte|格式符|輸出格式|
|:---:|:---:|:---:|:---:|
|char |1    |%c   |字元  |
|char |1    |%d   |整數  |

印出char可用%c或%d

{% highlight c++ linenos %}
char c = 'a';
printf("%d\n", c);
printf("%c\n", c);
{% endhighlight %}

```
執行結果
97
a
```

以上表格列出的整數資料型態都可以使用%d印出，只是印出的精準度不同。

## 浮點數

### 浮點數資料型態

|浮點數型態|byte|格式符|
|:---:|:---:|:---:|
|float|4|%f|
|double|8|%lf|
|long double|8|%Lf|

### float格式化輸出

{% highlight c++ linenos %}
float f1 = 3.8;
printf("f1 = %f \n", f1);
printf("f1 = %.2f \n", f1);
{% endhighlight %}

```
執行結果
f1 = 3.800000
f1 = 3.80
```

### double格式化輸出

{% highlight c++ linenos %}
float f1 = 3.8;
double d1 = 3.8;
printf("f1 = %f , d1 = %f , d1 = %lf\n", f1, d1, d1);
printf("f1 = %.2f , d1 = %.2f , d1 = %.2lf\n", f1, d1, d1);
{% endhighlight %}

```
執行結果
f1 = 3.800000 , d1 = 3.800000 , d1 = 3.800000
f1 = 3.80 , d1 = 3.80 , d1 = 3.80
```
