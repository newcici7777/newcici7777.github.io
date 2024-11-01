---
title: typedef類型別名
date: 2024-06-15
keywords: c++, typedef
---

## 不同作業系統基本資料型態記憶體大小

C++建立不同作業系統(跨平台)可以支援的資料型態。

|作業系統|short|int|long|long long|
|:--:|:--:|:--:|:--:|:--:|
|Linux|2byte|4byte|8byte|8byte|
|Windows|2byte|4byte|4byte|8byte|

由上表可以發現，Linux與Windows在long資料型態占用記憶體大小不同。

## 類型別名

為了解決long的資料型態在不同作業系統是不同記憶體大小，因此為類型取別名。

在寫程式時，資料型態使用類型別名，不再使用基本資料型態int，short，long，long long，避免不同作業系統資料型態記憶體大小不同的問題。

語法如下
```
typedef 資料型態	類型別名
```

## Windows類型別名

以下程式碼

int16_t是short的類型別名

int32_t是int的類型別名

int64_t是long long的類型別名


{% highlight c++ linenos %}
    //windows
    typedef short int16_t;//16位元整數
    typedef int int32_t;//32位元整數
    typedef long long int64_t;//64位元整數
{% endhighlight %}

## Linux類型別名

int16_t是short的類型別名

int32_t是int的類型別名

int64_t是long的類型別名

{% highlight c++ linenos %}
    //Linux
    typedef short int16_t;//16位元整數
    typedef int int32_t;//32位元整數
    typedef long int64_t;//64位元整數
{% endhighlight %}

## size_t 無符號整數類型

- 在32位元電腦size_t是unsigned int的類型別名
- 在64位元電腦size_t是unsigned long long的類型別名

|電腦|整數|byte數|範圍大小|
|:--:|:--:|:--:|
|32位元|unsigned int|4byte|0~4294967295|
|64位元|unsigned long long|8byte|0 到 18,446,744,073,709,551,615|

由上表可以發現不同位元的電腦，C++顯示的整數型態不一樣。

### 32位元的size_t類型別名

size_t是unsigned int類型別名

{% highlight c++ linenos %}
typedef unsigned int size_t;
{% endhighlight %}

### 64位元的size_t類型別名

size_t是unsigned long long類型別名

{% highlight c++ linenos %}
typedef unsigned long long size_t;
{% endhighlight %}