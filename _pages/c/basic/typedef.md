---
title: typedef 型別定義
date: 2024-06-15
keywords: c++, dynamic arrays, nothrow
---

## int16_t int32_t int64_t

C++建立不同作業系統(跨平台)可以支援的資料型態。

語法如下

```
typedef 資料型態	型別定義
```


|作業系統|short|int|long|long long|
|:--:|:--:|:--:|:--:|:--:|
|Linux|2byte|4byte|8byte|8byte|
|Windows|2byte|4byte|4byte|8byte|

由上表可以發現，Linux與Windows在long資料型態占用記憶體大小不同。

Windows
{% highlight c++ linenos %}
    //windows
    typedef short int16_t;//16位元整數
    typedef int int32_t;//32位元整數
    typedef long long int64_t;//64位元整數
{% endhighlight %}


Linux
{% highlight c++ linenos %}
    //Linux
    typedef short int16_t;//16位元整數
    typedef int int32_t;//32位元整數
    typedef long int64_t;//64位元整數
{% endhighlight %}

在寫程式時，資料型態使用int16_t與int32_t與int64_t，不再使用基本資料型態int，short，long，long long。

## size_t 無符號整數型態

|電腦|整數|byte數|範圍大小|
|:--:|:--:|:--:|
|32位元|unsigned int|4byte|0~4294967295|
|64位元|unsigned long long|8byte|0 到 18,446,744,073,709,551,615|

由上表可以發現不同位元的電腦，C++顯示的整數型態不一樣。

32位元定義的size_t
{% highlight c++ linenos %}
typedef unsigned int size_t;
{% endhighlight %}

64位元定義的size_t
{% highlight c++ linenos %}
typedef unsigned long long size_t;
{% endhighlight %}