---
title: 匿名函式就是lambda
date: 2025-05-14
keywords: kotlin, lambda
---
Prerequisites:

- [Java lambda][1]

匿名函式就是lambda

## 匿名函式是變數類型
匿名函式的類型是由「傳入的參數」與「傳回值類型」所決定的。

此處的匿名函式與C++的[函式指標][2]有相同的概念。

## 有名字的函式
有名字的函式要用return傳回值。
{% highlight kotlin linenos %}
fun sum(a: Int, b: Int): Int {
    return a + b
}
{% endhighlight %}

## 定義匿名函式

匿名函式不用有return，預設最後一行是傳回值。

變數的類型是匿名函式，匿名函式的類型是「傳入的參數類型」與「傳回值類型」。
{% highlight kotlin linenos %}
//  變數: (傳入的參數類型) -> 傳回值類型 = {參數: 類型, 參數: 類型 -> 傳回值}
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }
println(sum(10, 20));
{% endhighlight %}
```
30
```

## 推斷變數的類型
變數的類型是可以推斷的，就可以省略變數類型。
{% highlight kotlin linenos %}
//  變數 = {參數:類型,參數:類型 -> 傳回值}
val sum = { x: Int, y: Int -> x + y }
println("sum = ${sum(15, 25)}");
{% endhighlight %}
```
sum = 40
```

sendMsg1的類型是可以推斷，就可以省略變數類型。
{% highlight kotlin linenos %}
// 變數類型為() -> String
val sendMsg1 = {
    val appendMsg = "nice to meet you."
    "傳回訊息1 = $appendMsg"
}
println(sendMsg1())
{% endhighlight %}
```
傳回訊息1 = nice to meet you.
```

## 省略函式定義的參數類型
如果函式類型沒有省略，函式定義中的參數「類型」是可以省略。
{% highlight kotlin linenos %}
//  變數: (傳入的參數類型) -> 傳回值類型 = {參數, 參數 -> 傳回值}
val sum: (Int, Int) -> Int = { x, y -> x + y }
{% endhighlight %}

## 函式類型參數只有一個
{% highlight kotlin linenos %}
//  變數: (傳入的參數類型) -> 傳回值類型 = {參數 -> 傳回值}
val sendMsg: (String) -> String = { msg -> "傳回訊息 = $msg" }
println(sendMsg("Hello world!"))
{% endhighlight %}
```
傳回訊息 = Hello world!
```

## 使用it
簡化參數成it，只針對函式類型「參數只有一個」。
{% highlight kotlin linenos %}
val sendMsg: (String) -> String = { "傳回訊息 = $it" }
println(sendMsg("Hello world!"))
{% endhighlight %}
```
傳回訊息 = Hello world!
```

## 多行函式主體\{\}
{% highlight kotlin linenos %}
val sendMsg: (String) -> String = { 
    val appendMsg = "${it} nice to meet you."
    "傳回訊息 = $appendMsg" 
}
println(sendMsg("Hello world!"))
{% endhighlight %}
```
傳回訊息 = Hello world! nice to meet you.
```

## 函式參數是匿名函式1
{% highlight kotlin linenos %}
val callback: (Int) -> String = {
    when (it) {
        404 -> "找不到網頁"
        500 -> "Server error!"
        else -> "其它錯誤"
    }
}

val sendMsg3: (Int, (Int) -> String) -> String = 
    { code, function1 ->
        function1(code)
    }

println("函式的參數是函式，結果 = ${sendMsg3(404, callback)}")
{% endhighlight %}
```
函式的參數是函式，結果 = 找不到網頁
```

解釋:
- callback變數的函式類型定義是，參數是Int，傳回值是String。
- sendMsg3變數的函式類型定義是，傳回值是String類型，參數有二個，分別是Int與匿名函式。
- 匿名函式的類型定義是，參數是Int，傳回值是String類型。

{% highlight kotlin linenos %}
println("函式的參數是函式，結果 = ${sendMsg3(404, callback)}")
{% endhighlight %}
- 使用字串模板運算式`${}`
- 呼叫sendMsg3()函式
- 參數1，404
- 參數2，callback變數

## 函式參數是匿名函式2
{% highlight kotlin linenos %}
fun sendMsg4(code:Int, function1: (Int) -> String): String {
    return function1(code)
}

fun main() {
val callback: (Int) -> String = {
    when (it) {
        404 -> "找不到網頁"
        500 -> "Server error!"
        else -> "其它錯誤"
    }
}

println("有名字的函式的參數是函式，結果 = ${sendMsg4(500, callback)}");    
{% endhighlight %}
```
有名字的函式的參數是函式，結果 = Server error!
```

解釋:
- callback變數的函式類型定義是，參數是Int，傳回值是String。
- sendMsg4()定義在main()函式之外。
- sendMsg4()，傳回值是String類型，參數有二個，分別是Int與匿名函式。
- sendMsg4()是有名字的函式，要有return關鍵字。
- 匿名函式的類型定義是，參數是Int，傳回值是String類型。

## 最後一個參數是匿名函式
sendMsg5函式類型定義是，傳回值是Unit，沒有傳回值。

參數1是Int類型，參數2是匿名函式。

code是參數1的變數名，function1是參數2的變數名。
{% highlight kotlin linenos %}
val sendMsg5: (Int, (Int) -> String) -> Unit = 
    { code, function1 ->
        println("匿名函式在參數最後 = ${function1(code)}")
    }
{% endhighlight %}

把函式主體\{\}移出圓括號，花括號{}是匿名函式，是參2。
{% highlight kotlin linenos %}
sendMsg5(200) {
    if (it == 200) {
        "網頁正常"
    } else {
        "其它錯誤"
    }
}
{% endhighlight %}
```
匿名函式在參數最後 = 網頁正常
```

## 參數只有一個匿名函式，去掉圓括號
參數只有一個匿名函式。
{% highlight kotlin linenos %}
val sendMsg6: ((Int) -> String) -> Unit = {
    println("參數只有一個匿名函式 = ${it(200)}")
}
{% endhighlight %}

省略圓括號，花括號{}是匿名函式，是參數。
{% highlight kotlin linenos %}
sendMsg6 {
    if (it == 200) {
        "網頁正常"
    } else {
        "其它錯誤"
    }
}
{% endhighlight %}

## 透過匿名函式把標準函式重寫(覆寫)
{% highlight kotlin linenos %}
var str = "Hello World"
val o_count = str.count ({ letter ->
    letter == 'o'
})
{% endhighlight %}
- letter是參數。
- 只要計算字母為o的字元數量。
{% highlight kotlin linenos %}
letter == 'o'
{% endhighlight %}

簡化2，圓括號去掉
{% highlight kotlin linenos %}
var str = "Hello World"
val o_count = str.count { letter ->
    letter == 'o'
}
{% endhighlight %}

簡化3，letter與->箭頭去掉，因為只有一個參數，可以去掉，默認用it代替一個參數。
{% highlight kotlin linenos %}
var str = "Hello World"
val o_count = str.count {
    it == 'o'
}
{% endhighlight %}

印出字元個數
{% highlight kotlin linenos %}
var str = "Hello World"
println("字元個數 = ${str.count()}")
println("o字元個數 = ${o_count}")
{% endhighlight %}
```
字元個數 = 11
o字元個數 = 2
```

[1]: {% link _pages/java/lambda.md %}
[2]: {% link _pages/c/function/functionPointer.md %}