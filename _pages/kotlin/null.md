---
title: null
date: 2025-05-14
keywords: kotlin, null
---
## 非空值類型
在宣告變數時就能確定它是有值的，不能是null，若把變數設為null，會編譯錯誤。

非空值類型「不可以」儲存 null 值的變數。

{% highlight kotlin linenos %}
var name: String = "Tom" 
//null不能是非空值類型字符串的值 編譯錯誤
//name = null
{% endhighlight %}

## 空值類型
空值類型，是「可以」儲存 null 值的變數。

官方建議，用var宣告。

類型後面加上?，類型不可省略。
```
var 變數名:類型?
```

代表此變數「可以」儲存 null 值。
{% highlight kotlin linenos %}
var b: String? = "Tom"
b = null
{% endhighlight %}

這個變數的值有二種情況:值或null

使用這個變數時就必須額外檢查null，否則編譯器不會讓這個程式碼通過

## 存取空值變數的屬性

Kotlin 編譯器會對可為空值類型強制執行「null 檢查」。

以下程式碼會編譯失敗，因為沒有檢查空值類型。
{% highlight kotlin linenos %}
var b: String? = "Tom"
println(b.length)
{% endhighlight %}

## if null

在檢查的範圍內，可以使用變數的屬性。
{% highlight kotlin linenos %}
var b: String? = "Tom"
if (b != null) {
    println(b.length)
}
{% endhighlight %}
```
3
```

這樣做更好，若為null，就傳回0
{% highlight kotlin linenos %}
var b: String? = "Tom"
var result = if (b != null) {
  b.length
} else {
  0
}
println(result)
{% endhighlight %}

## 安全呼叫?.
使用?.符號來安全呼叫成員屬性。
{% highlight kotlin linenos %}
var b: String? = "Tom"
//如果B有值就存取它的length屬性
println(b?.length) 
{% endhighlight %}
```
3
```

## 安全呼叫?.存取null屬性
即使嘗試存取 null 變數的 length 屬性，該程式仍不會停止運作。安全呼叫運算式只會傳回 null。

語法
```
?.method
```

{% highlight kotlin linenos %}
var b: String? = null
println(b?.length) 
{% endhighlight %}
```
null
```

## 貓王(Elvis)運算子?:
什麼是貓王(Elvis)運算子?:

物件是null，傳回預設值。
```
變數 ?: 預設值
```

存取null物件的屬性，需搭配安全呼叫?.
```
變數?.屬性 ?: 預設值
```

{% highlight kotlin linenos %}
var name2:String? = null 
println(name2 ?: "no data")
println(name2?.length ?: "no data")
{% endhighlight %}
```
no data
no data
```

使用了一個安全呼叫?.後面的呼叫也必須使用安全呼叫，有一個呼叫是null，後面的安全呼叫就都不執行。
{% highlight kotlin linenos %}
val name3: String? = null
println(name3?.uppercase()?.replace("T","J")?.length ?: "name 3 is null")
{% endhighlight %}
```
name 3 is null
```

## let安全呼叫
即使嘗試存取 null 變數，該程式仍不會停止運作，安全呼叫運算式只會傳回 null。

若不為null，才會執行let{} Lambda函式，否則傳回null。
{% highlight kotlin linenos %}
val name3: String? = null
println(name3?.let { if (name3.isNotBlank()) name3 else "blank"  })
{% endhighlight %}
```
null
```

## 強制呼叫!!
確定有值，用在可為空值類型的變數，不打算使用安全呼叫，使用雙驚嘆號!!

語法
```
!!.method
```

若變數是null，就會產生 Exception in thread "main" java.lang.NullPointerException
{% highlight kotlin linenos %}
var c: String? = "Tom"
println(c!!.length)
{% endhighlight %}
