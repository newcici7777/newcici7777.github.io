---
title: 列舉
date: 2025-06-09
keywords: kotlin, enum
---
Prerequisites:

- [Java 列舉][1]

什麼是列舉？請詳見Java的列舉。

## 宣告語法
在kotlin宣告enum，跟Java略有不同，是用enum class 開頭。
```
enum class 類別名 {

}
```

## 主要建構式
在Kotlin，主要建構式不能使用name這個變數名。
{% highlight kotlin linenos %}
enum class Gender(val title : String) {
    GIRL("女孩"),
    BOY("男孩");
}
{% endhighlight %}

使用方法
{% highlight kotlin linenos %}
fun main() {
    println(Gender.BOY.title)
    println(Gender.GIRL.title)
}
{% endhighlight %}
```
男孩
女孩
```

## 沒有主要建構式
{% highlight kotlin linenos %}
enum class Gender {
    GIRL,
    BOY;
}
{% endhighlight %}

印出Enum的靜態屬性，預設印出屬性名，這部分跟Java是相同的。
{% highlight kotlin linenos %}
fun main() {
    println(Gender.BOY)
    println(Gender.GIRL)
}
{% endhighlight %}
```
BOY
GIRL
```

[1]: {% link _pages/java/enum.md %}
