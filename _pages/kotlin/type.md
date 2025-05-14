---
title: 變數與基本資料類型
date: 2025-05-12
keywords: kotlin, val, var, const, type
---
## 按2次shift鍵
輸入show kotlin bytecode 
![img]({{site.imgurl}}/kotlin/bytecode.png)

可以看到轉碼過後的java程式碼。

## 印出
{% highlight kotlin linenos %}
println()
{% endhighlight %}

## 結尾不用分號;
每一段程式碼後面不用加上分號;

## val唯讀變數
不可修改的變數。
{% highlight kotlin linenos %}
val str = "Hello World!"
println(str)
{% endhighlight %}

## var可變變數
變數是可以被改變，用var宣告參數
{% highlight kotlin linenos %}
var str = "Hello World!"
str = "Test"
println(str)
{% endhighlight %}

## const常數
只能在函式之外宣告並且定義，要寫在main()主函式外面，const常數只能使用基本資料類型String, Int, Double, Float, Long, Short, Char, Byte, Boolean
{% highlight kotlin linenos %}
const val MAX = 100
fun main() {
    println(MAX)
 }
{% endhighlight %}
```
100
```
## Unit
Unit 物件，等同java的void

## Any
Any 是kotlin中所有類別的根

## 基本資料類型
基本資料類型String, Int, Double, Float, Long, Short, Char, Byte, Boolean，字首全部大寫。

### 類型冒號空白
變數後面一個冒號，加上一個空白，接下來是類型。
{% highlight kotlin linenos %}
val str: String = "Test"
{% endhighlight %}

### 推斷類型
變數有初始值，類型可以從初始值推斷，就可以省略類型
{% highlight kotlin linenos %}
val str: String = "Hello"
// becomes
val str = "Hello"
{% endhighlight %}

### 數值類型
|整數類型|最小值|最大值|
|:---|:---|:---|
|Byte |-128 |127 |
|Short|-32768 |32767 |
|Int  |-2147483648(-2^31) |2147483647(-2^31-1) |
|Long |-2^63 |2^63-1 |

kotlin未超出Int，默認Int

kotlin超出Int最大值，推斷為Long類型

{% highlight kotlin linenos %}
val one = 1  //默認Int類型
val threeBillion = 30000000000 //Long類型 超出Int最大
val oneLong = 1L  //大寫L表示Long類型
val oneByte: Byte = 1 //指定為Byte的類型
{% endhighlight %}

### 基本類型轉換
不能不同類型隨便轉換，使用toXXX()才能轉換
```
toByte()
toShort()
toInt()
toLong()
toFloat()
toDouble()
toChar()
```

{% highlight kotlin linenos %}
val a: Int = 1
//val b: Long = a //error Type mismatch.
val b: Long = a.toLong()
{% endhighlight %}

{% highlight kotlin linenos %}
var a11: Int = 1
var c: Byte = 2
a11 = c.toInt()
{% endhighlight %}

{% highlight kotlin linenos %}
//字串與數字相連，先把數字toString，才能用+加號 連起來
val b1: String = "asd"
println(a11.toString() + b1)
// 現在都是用字串模板印出
println("${a11}${b1}")
{% endhighlight %}
```
2asd
2asd
```

## Float,Double
浮點類型有Float、Double

有小數點，編譯器默認為Double

若要指定為浮點數，後面要加f或F。

{% highlight kotlin linenos %}
val pi = 3.14 //Double
val e = 2.7182818384 //Double
val eFloat = 2.7182818384f //Float 但精度丟失 只到2.7182817
println(eFloat)
{% endhighlight %}
```
2.7182817
```

## Kotlin沒有自動轉型
{% highlight kotlin linenos %}
// 定義函式
// 函式參數只接收Double類型
fun printDouble(d: Double) {
    println(d)
}

var i = 1
val d = 1.1
var f = 1.1f

printDouble(d)
//printDouble(i)//error Type mismatch.
//printDouble(f)//error Type mismatch.
{% endhighlight %}
```
1.1
```

## Char
Char宣告單個字元，必須用''來表示。
{% highlight kotlin linenos %}
var char1: Char
char1 = 'a'
//數值不能塞char
//char1 = 1 //The integer literal does not conform to the expected type Char
{% endhighlight %}

在kotlin不能把字元拿來跟數字比
{% highlight kotlin linenos %}
val a1 = 'a'
//if(a1 == 97)  //Operator '==' cannot be applied to 'Char' and 'Int'
//必須寫 
if (a1.toInt() == 97) {
   println("Equal") 
}
{% endhighlight %}

Char轉成其它類型
{% highlight kotlin linenos %}
var char1: Char
char1 = 'a'
var var1 = char1.toByte()
var var2 = char1.toInt()
var var3 = char1.toString()
var var4 = char1.toFloat()
println("var1 => $var1")
println("var2 => $var2")
println("var3 => $var3")
println("var4 => $var4")
{% endhighlight %}
```
var1 => 97
var2 => 97
var3 => a
var4 => 97.0
```
## 字串
## 字串模板
把變數與運算結果輸出到螢幕上顯示。

### 印出變數
使用錢\$字號，後面是變數。
{% highlight kotlin linenos %}
val name: String = "Tom"
var age: Int = 18
age = 19
println("My name is $name, I'm $age")
{% endhighlight %}
```
hello
My name is Tom, I'm 19
```
### 印出運算結果
要用該變數的屬性或做任何的運算，只要額外使用\$錢字大括號{}把它包圍即可
{% highlight kotlin linenos %}
val name: String = "Tom"
var age: Int = 18
age = 19
println("My name have ${name.length} char")
println("1 + 2 = ${1 + 2}")
{% endhighlight %}
```
My name have 3 char
1 + 2 = 3
```

{% highlight kotlin linenos %}
val flag = false
println("Ans: ${if (flag) "ok" else "error"}")
{% endhighlight %}
```
Ans: error
```

### 印出排板
{% highlight kotlin linenos %}
val code = """ 
   val names = arrayOf("Tome","GG")
    |for(name in names)
     |println(name)
"""
println(code)
{% endhighlight %}
```
       val names = arrayOf("Tome","GG")
        |for(name in names)
         |println(name)
```

不要有多餘的縮排及空行，用trimMargin()函式消除

trimMargin使用行的開頭，默認使用\| 符號，做為每一行的開頭

{% highlight kotlin linenos %}
val code2 = """ 
   |val names = arrayOf("Tome","GG")
    |for(name in names)
     |println(name)
""".trimMargin()
println(code2)
{% endhighlight %}
```
val names = arrayOf("Tome","GG")
for(name in names)
println(name)
```
也可以換成其它的符號>，做為替換行的符號
{% highlight kotlin linenos %}
val code3 = """ 
   >val names = arrayOf("Tome","GG")
    >for(name in names)
     >println(name)
""".trimMargin(">")
println(code3)
{% endhighlight %}
```
val names = arrayOf("Tome","GG")
for(name in names)
println(name)
```