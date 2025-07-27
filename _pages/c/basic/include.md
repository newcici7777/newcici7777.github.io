---
title: include
date: 2025-01-13
keywords: c++, include
---
## head file
檔名以.h為結尾，定義使用的標頭檔，定義(definition)函式，定義類別，定義結構...等等。

## include head file
從編譯器中的Library找尋標頭檔:
{% highlight c++ linenos %}
#include <標頭檔>
{% endhighlight %}

檔案所在目錄下，尋找副檔名.h標頭檔，找不到就去編譯器中的Library找尋。
{% highlight c++ linenos %}
#include "標頭檔.h"
{% endhighlight %}

include也可以用.cpp檔，不是只限於.h

include是把檔案內容複製下來，貼上呼叫include的位置，跟[內嵌函式][1]是一樣的。

## pragma once
重覆include的標頭檔，只要include一次，放在檔案最上面。
{% highlight c++ linenos %}
#pragma once
{% endhighlight %}

## C Standard Library
c98的Standard Library，舊版本會加上.h，新版本前面會加上c去掉.h

不加上std的namespace也可以使用C Standard Library

舊版本
{% highlight c++ linenos %}
#include <stdio.h>
{% endhighlight %}

新版本前面會加上c去掉.h
{% highlight c++ linenos %}
#include <cstdio>
{% endhighlight %}

## C++ Standard Library
要加上std的namespace才可以使用C++ Standard Library
{% highlight c++ linenos %}
using namespace std;
{% endhighlight %}

舊版本會加上.h，新版本會去掉.h，已棄用。
{% highlight c++ linenos %}
#include <iostream.h>
{% endhighlight %}

新版本會去掉.h
{% highlight c++ linenos %}
#include <iostream>
{% endhighlight %}




