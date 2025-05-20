---
title: if, when, while
date: 2025-05-14
keywords: kotlin, when, if, while ,for
---
## if 條件運算子
{% highlight kotlin linenos %}
val b: Any = 1
val res3: Boolean = if (b == 1) true else false
println("res3:$res3")  
{% endhighlight %} 
```
res3:true
```

## if 接收傳回值 
什麼時候會判斷if是要有傳回值的？

拿變數去接收它會自動判斷if是要去獲取傳回值的。

但一定要帶上else，因為它要判斷如果if條件不相等，要帶上其它值。

不用有return，預設程式碼區塊\{\}的最後一行是傳回值。
{% highlight kotlin linenos %}
val b: Any = 1
val res: Int = if (b == 1) {
    3
} else if (b == 3) {
    1
} else {
    2
}
println(res)  // 3
{% endhighlight %}

{% highlight kotlin linenos %}
val count = 42
val answerString: String = if (count == 42) {
    "I have the answer."
} else if (count > 35) {
    "The answer is close."
} else {
    "The answer eludes me."
}
println(answerString)
{% endhighlight %}
```
I have the answer.
```

## if條件語句不只有一行，省略return
如果if條件語句內的程式碼不只一行，必須把結果放在最後一行，而且不可以加return。

使用if條件語句作為傳回值的時候，一定要有else區塊，確保有值，才能指派給變數
{% highlight kotlin linenos %}
var a = 3
val b = 9
val chooseMax = if (a > b) {
    println("a最大")
    a //結果值放在最後一行，不要加return
} else {
    println("b最大")
    b
}
println("chooseMax = $chooseMax")    
{% endhighlight %}
```
b最大
chooseMax = 9
```

## when
### 程式碼區塊
when每個程式碼區塊\{\}是用 -> 箭頭指向程式碼區塊{}，預設程式碼區塊\{\}的最後一行是傳回值。
{% highlight kotlin linenos %}
val str: Any = "abc"
when(str) {
    // 判斷b == 1 or b== 2，可以寫在同一排
    1, 2 -> {  
        println("是1")
    }
    // 要把c設成Any，就不會自動推導Int，若沒設Any，會有型別錯誤
    is String -> {  
        println(" is str")
    }
    // 判斷是不是在1到10的區間中
    in 1 .. 10 -> {  
        println(" 1 到 10 ")
    }
    else -> {
        println("沒有東西")
    }
}
{% endhighlight %}

如果只有一行，就不用寫花括號，if也是一樣。
{% highlight kotlin linenos %}
val b:Any = 1
println(
    when (b) {
        1, 2 -> 2
        is String -> 3
        in 1 .. 10 -> 4
        else -> 5
    }
)
{% endhighlight %}
```
2
```

when的條件判斷不會像java的switch自動向下執行，所以不必使用break來終止。

當if - else條件過多時，建議改用when。

注意x變數的類型是Any。
{% highlight kotlin linenos %}
var x: Any = 1
when {
    // x 等於1的時候執行，箭頭表示符合時要如何處理
    x == 1 -> println("x 是 1")
    // x 等於2,3或者等於4的時候
    x == 2 || x == 3 || x == 4 -> println("x 可能是2 3 4")
    // x 在 5-10 的時候執行
    x in 5 .. 10 -> println("x 在 5-10 的時候執行")
    // x 是 int的類型
    x is Int -> println("x是整數")
    // 其它情況
    else -> println("無法判斷")
}
{% endhighlight %}
```
x 是 1
```

把x放在when的參數裡。
{% highlight kotlin linenos %}
var x: Any = 1
when (x) {
    1 -> println("x 是 1")
    2, 3, 4 -> println("x 可能是2 3 4")
    in 5 .. 10 -> println("x 在 5-10 的時候執行")
    is Int -> println("x是整數")
    else -> println("無法判斷")
    // 如果所有可能條件都列出時，可以省略else
}
{% endhighlight %}
```
x 是 1
```

拿message變數去接收when。
{% highlight kotlin linenos %}
var x: Any = 1
val message = when(x) {
    1 -> println("x 是 1")
    2, 3, 4 -> println("x 可能是2 3 4")
    in 5 .. 10 -> println("x 在 5-10 的時候執行")
    is Int -> println("x是整數")
    else -> {
        // 可以寫很多東西，要記得把結果放在最後一行
        println("無法判斷")
    }
}
{% endhighlight %}
```
x 是 1
```

拿變數去接收條件語句會自動判斷when是要去獲取傳回值的。

但一定要有else，若之前的條件都不滿足，要有個else取得默認的傳回值。

注意，b變數是Any類型，預設程式碼區塊\{\}的最後一行是傳回值。
{% highlight kotlin linenos %}
val b: Any = 1
val res2 = when(b) {
    // 判斷b == 1 or b== 2，可以寫在同一排
    1, 2 -> { 
        2
    } 
    // 要把b設成Any，就不會自動推導Int，若沒設Any，會有型別錯誤
    is String -> { 
        3
    } 
    // 判斷是不是在1到10的區間中
    in 1 .. 10 -> { 
        4
    } 
    else -> {
        5
    }
}
println(res2)  // 2
{% endhighlight %}

## for
### continue
{% highlight kotlin linenos %}
//跳過4
for (i in 1..10) {
    if (i == 4) continue
    println(i)
}
{% endhighlight %}
```
1
2
3
5
6
7
8
9
10
```

### break
{% highlight kotlin linenos %}
for (i in 1..10) {
    if (i == 5) break;
    println(i)
}
{% endhighlight %}
```
1
2
3
4
```

## foreach
{% highlight kotlin linenos %}
(0 .. 5).forEach {
    println(it)
}
{% endhighlight %}
```
0
1
2
3
4
5
```

## repeat
重覆執行某段程式碼一定的次數
{% highlight kotlin linenos %}
repeat(3) {
    println("hello")
}
{% endhighlight %}
```
hello
hello
hello
```

## while
{% highlight kotlin linenos %}
var i = 0
while (i < 6) {
    println(i)
    i++
}
{% endhighlight %}
```
0
1
2
3
4
5
```

{% highlight kotlin linenos %}
i = 0
while (i < 6) println(i++)
{% endhighlight %}
```
0
1
2
3
4
5
```

加1放在前面表示先加1再加上自己
{% highlight kotlin linenos %}
i = 0
while (i < 6) println(++i)
{% endhighlight %}
```
1
2
3
4
5
6
```

即使條件不滿足，也至少執行一次
{% highlight kotlin linenos %}
i = 6
do {
    println(i)
    i++
} while (i < 6)
{% endhighlight %}
```
6
```

## 標籤\@
某些情況下想離開外面的那個迴圈

### break標籤\@
標籤名稱@ 指定要跳離的迴圈
{% highlight kotlin linenos %}
i = 0
abc@ do {
    println("out loop $i")
    i++
    var j = 0
    while (j < 3) {
        println("-- in loop $j")
        j++
        break@abc
    }
} while (i < 6)
{% endhighlight %}
```
out loop 0
-- in loop 0
```
外迴圈、內迴圈只跑一次

### continue標籤\@
{% highlight kotlin linenos %}
i = 0
outer@ do {
    println("out loop $i")
    i++
    var j = 0
    while (j < 3) {
        println("-- in loop $j")
        j++
        continue@outer
    }
} while (i < 6)
{% endhighlight %}
```
out loop 0
-- in loop 0
out loop 1
-- in loop 0
out loop 2
-- in loop 0
out loop 3
-- in loop 0
out loop 4
-- in loop 0
out loop 5
-- in loop 0
```
外迴圈跑6遍，內迴圈參與一次

### return標籤\@
類似continue功能。

不要印出2，遇到2就continue下一個for loop，其它的都要印。
{% highlight kotlin linenos %}
(0 .. 10).forEach b@{
    if(it == 2)
        return@b
    println(it)
}
{% endhighlight %}
```
1
3
4
5
6
7
8
9
10
```
以上沒有印2

### run標籤\@
跳離forEach

要跳出整個foreach如何使用？使用run, run也是lambda ,結束run這個循環在run的循環加上標注
{% highlight kotlin linenos %}
run c@{
    (0 .. 10).forEach {
        if(it == 2)
            return@c
        println(it)
    }
}
{% endhighlight %}
```
0
1
```

## return
return 會跳離迴圈最接近的函式。

不管是否為巢狀迴圈，永遠會跳離最接近的函式。
{% highlight kotlin linenos %}
fun foo(){
    for (i in 0..1) {
        println("for i start: $i")
        for (j in 0 .. 3) {
            println("for j start: $j")
            if (j == 2) {
                println("out foo()")
                return
            }
            println("for j finish: $j")
        }
        println("for i finish $i")
    }
    println("finish foo()")
}

foo()
{% endhighlight %}
```
for i start: 0
for j start: 0
for j finish: 0
for j start: 1
for j finish: 1
for j start: 2
out foo()
```


