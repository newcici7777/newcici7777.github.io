---
title: null
date: 2025-05-14
keywords: kotlin, null
---
## 非空類型
在宣告變數時就能確定它是有值的，不能是null，若把變數設為null，會編譯錯誤。
{% highlight kotlin linenos %}
var name: String = "Tom" 
//null不能是非空類型字符串的值 編譯錯誤
//name = null
{% endhighlight %}

## 可空類型
類型後面加上?，類型不可省略。
```
變數名:類型?
```
表示此變數接受null值
{% highlight kotlin linenos %}
val b: String? = "Tom"
b = null
{% endhighlight %}

這個變數的值有二種情況:值或null

使用這個變數時就必須額外檢查null，否則編譯器不會讓這個程式碼通過

## 檢查可空類型
以下程式碼會編譯失敗，因為沒有檢查可空類型。
{% highlight kotlin linenos %}
val b: String? = "Tom"
println(b.length)
{% endhighlight %}

isNotEmpty可以檢查null，在檢查的範圍內，可以使用變數的屬性。
{% highlight kotlin linenos %}
val b: String? = "Tom"
if (b != null && b.isNotEmpty()) {
    // 使用length屬性
    println(b.length)
}
{% endhighlight %}
```
3
```

## 安全呼叫?.
使用?.符號來安全呼叫成員屬性。
{% highlight kotlin linenos %}
val b: String? = "Tom"
//如果B有值就存取它的length屬性
println(b?.length) 
{% endhighlight %}
```
3
```

## 貓王(Elvis)運算符號?:
使用了一個安全呼叫?.在其後的呼叫也必須使用安全呼叫，有一個是null，其後的都不執行。

什麼是貓王(Elvis)運算符號?:

得到null結果時指定預設值。
{% highlight kotlin linenos %}
val b: String? = "Tom"
println(b?.uppercase()?.replace("T","J")?.length ?: "is null")
{% endhighlight %}

## 強制呼叫!!
確定有值，不打算使用安全呼叫，使用雙驚嘆號!!

若變數是null，就會產生 Exception in thread "main" java.lang.NullPointerException
{% highlight kotlin linenos %}
var c: String? = "Tom"
println(c!!.length)
{% endhighlight %}
