---
title: Lambda
date: 2025-05-14
keywords: kotlin, Lambda
---
Prerequisites:

- [Java Lambda][1]

Lambda就是匿名函式，沒有名字的函式。

## Lambda是變數類型
Lambda的類型是由「傳入的參數」與「傳回值類型」所決定的。

此處的Lambda與C++的[函式指標][2]有相同的概念。

## 有名字的函式
有名字的函式要用return傳回值。
```
fun 函式名(參數): 傳回值類型 {
    return 傳回值
}
```
{% highlight kotlin linenos %}
fun sum(a: Int, b: Int): Int {
    return a + b
}
{% endhighlight %}

## 沒有名字的函式Lambda
用花括號{}開始跟結束，使用->將參數跟函式內容分開，參數可以1個、多個、或沒有。

函式內容可以有多行，最後計算的運算式作為Lambda傳回值，例如下面程式碼傳回x + 5的結果。

->用於分隔任何參數。

->後面要傳回的結果，Lambda不用有return。

如果Lambda沒有參數，可以省略->

Lambda
{% highlight kotlin linenos %}
{ x: Int -> "The value is $x" }
{% endhighlight %}

把Lambda指派給變數，並執行。
{% highlight kotlin linenos %}
val msg = { x: Int -> "The value is $x" }
// 執行Lambda有代入參數，要用${運算式}
println("pass 6 to msg:${msg(6)}")
{% endhighlight %}
```
pass 6 to msg:The value is 6
```

## Lambda類型
變數的類型是Lambda，Lambda類型是「傳入的參數類型與個數」+「傳回值類型」所組合。

Lambda可以指派給變數，以下把`{ x: Int, y: Int -> x + y }`指派給變數sum。

![img]({{site.imgurl}}/kotlin/lambda1.png)

\{\}大括號是函式主體body，函式主體中有「參數」與「傳回值」，與Lambda類型是一致的。
{% highlight kotlin linenos %}
//  變數: (傳入的參數類型) -> 傳回值類型 = {參數: 類型, 參數: 類型 -> 傳回值}
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }
println(sum(10, 20));
{% endhighlight %}
```
30
```

## 自動推導Lambda類型
變數的Lambda類型是可以自動推導的，就可以省略Lambda類型。
{% highlight kotlin linenos %}
// 省略Lambda類型(Int, Int) -> Int
val sum = { x: Int, y: Int -> x + y }
// 有帶參數記得加上花括號印出${}
println("sum = ${sum(15, 25)}");
{% endhighlight %}
```
sum = 40
```

sendMsg1的Lambda類型是可以自動推導，就可以省略Lambda類型。
{% highlight kotlin linenos %}
// 省略Lambda類型() -> String
val sendMsg1 = {
    val appendMsg = "nice to meet you."
    "傳回訊息1 = $appendMsg"
}
println(sendMsg1())
{% endhighlight %}
```
傳回訊息1 = nice to meet you.
```

## 省略函式主體參數類型
如果Lambda類型沒有省略，\{\}大括號稱為函式主體body，函式主體中的參數「類型」是可以省略。
{% highlight kotlin linenos %}
//  變數: (傳入的參數類型) -> 傳回值類型 = {參數, 參數 -> 傳回值}
val sum: (Int, Int) -> Int = { x, y -> x + y }
{% endhighlight %}

## 參數只有一個，使用it
參數只有一個。
{% highlight kotlin linenos %}
//  變數: (傳入的參數類型) -> 傳回值類型 = {參數 -> 傳回值}
val sendMsg: (String) -> String = { msg -> "傳回訊息 = $msg" }
println(sendMsg("Hello world!"))
{% endhighlight %}
```
傳回訊息 = Hello world!
```

省略msg參數，省略->箭頭，把\$msg變成\$it，只針對「參數只有一個」。
{% highlight kotlin linenos %}
val sendMsg: (String) -> String = { "傳回訊息 = $it" }
println(sendMsg("Hello world!"))
{% endhighlight %}
```
傳回訊息 = Hello world!
```

## 多行程式碼，最後一行是傳回值
在函式主體\{\}中，可以有多行程式碼，最後一行是傳回值。
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

## Lambda參數是Lambda
Lambda也是匿名函式，匿名函式的參數是匿名函式
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

println("Lambda參數是Lambda，結果 = ${sendMsg3(404, callback)}")
{% endhighlight %}
```
Lambda參數是Lambda，結果 = 找不到網頁
```

解釋:
- callback變數的Lambda類型是，參數是Int，傳回值是String。
- sendMsg3變數的Lambda類型是，參數有二個，分別是Int與Lambda，傳回值是String類型。
- sendMsg3()，參數2的Lambda類型是`(Int) -> String`，參數是Int，傳回值是String。

{% highlight kotlin linenos %}
println("Lambda參數是Lambda，結果 = ${sendMsg3(404, callback)}")
{% endhighlight %}
- 使用字串模板運算式`${}`
- 呼叫sendMsg3()函式
- 參數1，404
- 參數2，callback變數

## 有名字的函式參數是Lambda
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

println("有名字的函式的參數是Lambda，結果 = ${sendMsg4(500, callback)}");    
{% endhighlight %}
```
有名字的函式的參數是Lambda，結果 = Server error!
```

解釋:
- callback變數的Lambda類型是，參數是Int，傳回值是String。
- sendMsg4()定義在main()函式之外。
- sendMsg4()，傳回值是String類型，參數有二個，分別是Int與Lambda。
- sendMsg4()，參數2的Lambda的類型是`(Int) -> String`，參數是Int，傳回值是String類型。
- sendMsg4()是有名字的函式，要有return關鍵字。

## 最後一個參數是Lambda
- sendMsg5()Lambda類型是，參數1是Int類型，參數2是Lambda，傳回值是Unit，沒有傳回值。
- sendMsg5()，參數2的Lambda的類型是`(Int) -> String`，參數是Int，傳回值是String類型。

code是參數1的變數名，function1是參數2的變數名。
{% highlight kotlin linenos %}
val sendMsg5: (Int, (Int) -> String) -> Unit = 
    { code, function1 ->
        println("Lambda在參數最後 = ${function1(code)}")
    }
{% endhighlight %}

把函式主體\{\}移出圓括號，花括號{}是Lambda，是參數2。
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
Lambda在參數最後 = 網頁正常
```
## 參數只有一個Lambda，使用it()呼叫Lambda函式。
sendMsg6()，參數1的Lambda類型為(Int) -> String，參數是Int，傳回值是String類型。

參數只有一個Lambda，使用it代表Lambda函式。<br>
it代表的是以下的Lambda函式宣告，函式參數為Int，傳回值是String。
```
(Int) -> String
```

`it(200)`，對函式傳入參數200。<br>
{% highlight kotlin linenos %}
val sendMsg6: ((Int) -> String) -> Unit = {
    println("參數只有一個Lambda = ${it(200)}")
}
{% endhighlight %}

## 參數只有一個Lambda，去掉圓括號
sendMsg6()，參數1的Lambda類型為(Int) -> String，參數是Int，傳回值是String類型。
{% highlight kotlin linenos %}
val sendMsg6: ((Int) -> String) -> Unit = {
    println("參數只有一個Lambda = ${it(200)}")
}
{% endhighlight %}

函式宣告如下，因為參數也只有一個(Int)，it代表(Int)這個參數
```
(Int) -> String
```

未省略圓括號前。
{% highlight kotlin linenos %}
sendMsg6 ({
    if (it == 200) {
        "網頁正常"
    } else {
        "其它錯誤"
    }
})
{% endhighlight %}

省略圓括號，花括號{}是Lambda，是參數。
{% highlight kotlin linenos %}
sendMsg6 {
    if (it == 200) {
        "網頁正常"
    } else {
        "其它錯誤"
    }
}
{% endhighlight %}

完整程式碼
{% highlight kotlin linenos %}
val sendMsg6: ((Int) -> String) -> Unit = {
    println("參數只有一個Lambda = ${it(200)}")
}

sendMsg6 {
    if (it == 200) {
        "網頁正常"
    } else {
        "其它錯誤"
    }
}
{% endhighlight %}
```
參數只有一個Lambda = 網頁正常
```

## 傳回值是Lambda
以下是有名字的函式，所以會用到return，注意，它的傳回值是Lambda類型(Int) -> String，函式參數是url。
{% highlight kotlin linenos %}
fun sendMsg7(url: String) : (Int) -> String {
    val contact = "xxx@xxx.mail.com"
    return { code: Int ->
        when (code) {
            404 -> "${url} 找不到網頁, please contact ${contact}"
            500 -> "${url} Server error!, please contact ${contact}"
            else -> "${url} 其它錯誤, please contact ${contact}"
        }
    }
}
{% endhighlight %}

傳回值解說:

\{\}大括號就是Lambda主體body，參數是code，會傳回String，Lambda可以用外部函式sendMsg7()的參數url與變數contact。
{% highlight kotlin linenos %}
    return { code: Int ->
        when (code) {
            404 -> "${url} 找不到網頁, please contact ${contact}"
            500 -> "${url} Server error!, please contact ${contact}"
            else -> "${url} 其它錯誤, please contact ${contact}"
        }
    }
{% endhighlight %}

msgfun變數接收到的是傳回值函式，就是上面大括號{}包住的程式碼，傳回值函式的類型是(Int) -> String。  
msgfun(404)是把參數代入，傳回值是String
{% highlight kotlin linenos %}
fun main() {
    val msgfun = sendMsg7("http://www.xxx.com")
    println(msgfun(404))
}
{% endhighlight %}
```
http://www.xxx.com 找不到網頁, please contact xxx@xxx.mail.com
```

![img]({{site.imgurl}}/kotlin/lambda2.png)

## 參數沒使用，用_代表
{% highlight kotlin linenos %}
val useLambda2: (Int, Int) -> Unit = { x, _ ->
    println("First:$x")
}
useLambda2(6, 9)
{% endhighlight %}
```
First:6
```

## 透過Lambda把標準函式重寫(覆寫)
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