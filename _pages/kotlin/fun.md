---
title: 函式
date: 2025-05-14
keywords: kotlin, fun
---
## 函式宣告
以fun開頭後面接函式名，有函式名的稱為有名字的函式。

有名字的函式要用return傳回值。
```
fun 函式名(參數: 參數類型): 傳回值類型 {
    return 傳回值
}
```

多參數
```
fun 函式名(參數1: 參數類型, 參數2: 參數類型): 傳回值類型 {
    return 傳回值
}
```

以下程式碼圓括號裡是參數`(參數1: 參數類型, 參數2: 參數類型)`。

`: Int`為傳回值型別。
{% highlight kotlin linenos %}
fun sum(a: Int, b: Int): Int {
    return a + b
}
fun main() {
    // 有帶參數記得加上花括號印出${}
    println("result =  ${sum(1, 2)}")
}
{% endhighlight %}
```
result =  3
```

## 簡化
如果只有一行，省略花括號\{\}，省略return。
{% highlight kotlin linenos %}
fun a(): Int {
    return 1
}
{% endhighlight %}

使用等於=
{% highlight kotlin linenos %}
fun b():Int = 1
{% endhighlight %}

若傳回值類型可以自動推導，可以省略傳回值類型。
{% highlight kotlin linenos %}
fun c() = 1
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
參數可以有預設值。
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

## 函式參考
使用2個冒號::後面是有名字的函式，呼叫函式時，有帶參數記得加上花括號印出${}
{% highlight kotlin linenos %}
fun calculate(x: Int): Int {
    return x + 3
}
val funRef = ::calculate
// 有帶參數記得加上花括號印出${}
println("result =  ${funRef(6)}")
{% endhighlight %}
```
9
```

## 匿名函式
沒有名字的函式，沒有函式名，只有fun關鍵字。
{% highlight kotlin linenos %}
val useAnonymousFun = fun(x: Int): Int {
    return x + 3
}
//有帶參數記得加上花括號印出${}
println("use AnonymousFun:${useAnonymousFun(6)}")
{% endhighlight %}
```
9
```

## inline
在fun關鍵字前面加上inline，意思是編譯的時候把程式碼複製到呼叫的位置。
{% highlight kotlin linenos %}
inline fun calculate(x: Int): Int {
    return x + 3
}
fun main() {
    println("inline function1 = ${calculate(5)}")
    println("inline function2 = ${calculate(6)}")
    println("inline function3 = ${calculate(7)}")
}
{% endhighlight %}

按2次shift鍵，輸入show kotlin bytecode

按下「Decompile」按鈕，可以看到轉碼過後的java程式碼。

![img]({{site.imgurl}}/kotlin/inline.png)

為什麼要使用inline呢？因為每個函式都會占記憶體，使用inline直接把函式中的程式碼複製過去，可以減少函式記憶體，但這個函式的內容不多才可以這樣做。

## 私有函式
預設是public，全部的.kt文件都可以使用，若要變成私有，前面要加上private，函式只能被所在的位置.kt文件使用，不同文件.kt文件不能用。
{% highlight kotlin linenos %}
private fun sum(a: Int, b: Int): Int {
    return a + b
}
{% endhighlight %}
