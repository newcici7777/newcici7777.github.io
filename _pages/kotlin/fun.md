---
title: 函式
date: 2025-05-14
keywords: kotlin, fun
---
## 函式宣告
以fun開頭後面接函式名。

圓括號裡是參數`(參數1: 參數類型, 參數2: 參數類型)`。

`: Int`為傳回值型別。
{% highlight kotlin linenos %}
fun sum(a: Int, b: Int): Int {
    return a + b
}
fun main() {
    println(sum(1, 2))
}
{% endhighlight %}
```
3
```

## 私有函式
預設是public，全部的.kt文件都可以使用，若要變成私有，前面要加上private，函式只能被所在的位置.kt文件使用，不同文件.kt文件不能用。
{% highlight kotlin linenos %}
private fun sum(a: Int, b: Int): Int {
    return a + b
}
{% endhighlight %}

## 用等於\=來指定傳回值的運算式
用 = 來指定傳回值的運算式
{% highlight kotlin linenos %}
fun plus(a: Int, b: Int) : Int = a + b
fun main() {
    println(plus(1,2))
}
{% endhighlight %}
```
3
```

## 參數預設值
{% highlight kotlin linenos %}
fun sum2(a: Int = 0, b: Int = 3, c: Int){
    println(a + b + c)
}
fun main() {
    // 使用參數名稱指定值，略過有預設值的B
    sum2(a = 3, c = 6)
    // 略過有預設值的a和b
    sum2(c = 5) 
    // 以名稱指定參數的值，就可以忽略原始參數的順序
    sum2(c = 9, a = 1, b = 3 ) 
}
{% endhighlight %}
```
12
8
13
```

## Unit函式
Unit函式指的是沒有傳回值的函式，等同java的void，但java的void不是類型，傳回值Unit是一種類型。
{% highlight kotlin linenos %}
//無參數，無傳回值的函式
fun foo() {}
fun main() {
    println(foo())
}
{% endhighlight %}
```
kotlin.Unit
```



