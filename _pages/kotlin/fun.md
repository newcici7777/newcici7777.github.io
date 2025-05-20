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

## 指派
用 = 來指派傳回值的運算式
{% highlight kotlin linenos %}
fun plus(a: Int, b: Int) : Int = a + b
fun main() {
    println(plus(1,2))
}
{% endhighlight %}
```
3
```

## if在函式中省略return過程
if在函式有return
{% highlight kotlin linenos %}
fun chooseMax(a:Int, b:Int) : Int {
    if(a > b) {
        return a
    } else {
        return b
    }
}
fun main() {
    println("chooseMax fun = ${chooseMax(10, 20)}")
}
{% endhighlight %}
```
chooseMax fun = 20
```

if 條件運算子
{% highlight kotlin linenos %}
fun chooseMax2(a:Int, b:Int):Int{
    return if (a > b) a else b
}
fun main() {
    println("chooseMax2 fun = ${chooseMax2(10, 20)}")
}
{% endhighlight %}
```
chooseMax2 fun = 20
```

省略return，用指派=
{% highlight kotlin linenos %}
fun chooseMax3(a:Int, b:Int) = if (a > b) a else b
fun main() {
    println("chooseMax3 fun = ${chooseMax3(10, 20)}")
}
{% endhighlight %}
```
chooseMax3 fun = 20
```

if多行條件語句，用指派，省略return。
{% highlight kotlin linenos %}
fun chooseMax4(a:Int, b:Int) = if (a > b) {
    println("a最大")
    a //結果值放在最後一行，不要加return
} else {
    println("b最大")
    b
}
fun main() {
    println("chooseMax4 fun = ${chooseMax4(10, 20)}")
}
{% endhighlight %}
```
b最大
chooseMax4 fun = 20
```

## 函式中的函式
函式中的函式可以存取外部函式的變數，但外部函式無法存取內部函式的變數。

func_level1()函式無法使用func_level2()與func_level3()裡面的變數。
{% highlight kotlin linenos %}
fun main() {
    val level0 = 0
    fun func_level1() {
        println("level0 = $level0")
        val level1 = 1
        fun func_level2() {
            println("=============")
            println("level0 = $level0")
            println("level1 = $level1")
            val level2 = 2
            fun func_level3() {
                println("=============")
                println("level0 = $level0")
                println("level1 = $level1")
                println("level2 = $level2")
            }
            func_level3()
        }
        func_level2()
    }
    func_level1()
}
{% endhighlight %}
```
level0 = 0
=============
level0 = 0
level1 = 1
=============
level0 = 0
level1 = 1
level2 = 2

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
