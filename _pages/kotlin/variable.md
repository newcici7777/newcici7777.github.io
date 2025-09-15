---
title: vararg函式可變參數
date: 2025-09-12
keywords: kotlin, vararg, variable parameter 
---
## 語法
```
fun 函式名(vararg 參數名:參數類型)
fun func1(vararg s:Int)
```

使用`[索引]`取出值。
{% highlight kotlin linenos %}
fun func1(vararg s:Int) {
    println(s[0])
}

fun main() {
    func1(10, 20, 30)
}
{% endhighlight %}
## vararg參數
### 多參數
Kotlin的可變參數vararg放在最後一個參數。<br>
{% highlight kotlin linenos %}
fun func1(arg1:Int, vararg arg2:Int) {
    println("arg1 = $arg1")
    for (i in arg2) {
        println(i)
    }
}

fun main() {
    func1(1000, 10, 20, 30)
}
{% endhighlight %}
```
arg1 = 1000
10
20
30
```
### 其它參數有預設值，且沒有傳入參數
arg1有預設值為0，若函式呼叫時沒代入arg1的參數。<br>
arg2要有參數名，且要把可變參數轉型成陣列，如以下程式是intArrayOf(可變參數...)<br>
{% highlight kotlin linenos %}
fun func1(arg1:Int = 0, vararg arg2:Int) {
    println("arg1 = $arg1")
    for (i in arg2) {
        println(i)
    }
}

fun main() {
    func1(arg2 = intArrayOf(10, 20, 30))
}
{% endhighlight %}
```
arg1 = 0
10
20
30
```
### vararg不為最後一個參數
vararg不為最後一個，後面有參數，後面參數傳入時要有參數名。<br>
```
                 在vararg後面的參數要有參數名
                  ↓
func1(10, 20, 30, arg2 = "Hello")
```

{% highlight kotlin linenos %}
fun func1(vararg arg1:Int, arg2:String) {
    println("arg2 = $arg2")
    for (i in arg1) {
        println(i)
    }
}

fun main() {
    func1(10, 20, 30, arg2 = "Hello")
}
{% endhighlight %}
```
b = 20
10
20
30
```

