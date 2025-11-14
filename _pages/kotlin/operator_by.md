---
title: by 運算子
date: 2025-11-14
keywords: kotlin, by operator
---
- [委托][1]

Kotlin 的 「屬性」 by 委託，是用來把「屬性的 getter / setter 行為」交給另一個物件處理。

|運算子|	覆寫方法|	說明|
|:---|:-----|:-----|
|by|	getValue / setValue|	委托|

使用 by 關鍵字，委托的類別要實作以下二個方法。<br>
```
operator fun getValue(...)
operator fun setValue(...)
```
- getValue：當你讀取屬性時會被呼叫
- setValue：當你寫入屬性時會被呼叫（只針對 var，val 沒有 setValue）

{% highlight kotlin linenos %}
import kotlin.reflect.KProperty

class Student {
    var address: String by AddressDelegate()
}

class AddressDelegate {
    // 需要有一個暫存變數temp，儲存變數的值
    // 變數預設值為no data
    private var temp = "no data"
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        println("$thisRef ,讀取屬性 ${property.name}")
        // get()的時候，傳回暫存變數
        return temp
    }
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String){
        // 設定暫存變數temp
        temp = value
        println("$thisRef , 設定屬性 ${property.name} change to $value")
    }
}

fun main() {
    val student = Student()
    // call AddressDelegate getValue()
    println(student.address)
    // call AddressDelegate setValue()
    student.address = "Taiwan"
    // call AddressDelegate getValue()
    println(student.address)
}
{% endhighlight %}
```
no data
learn2.Student@4783da3f , 設定屬性 address change to Taiwan
learn2.Student@4783da3f , 讀取屬性 address
Taiwan
```

[1]: {% link _pages/kotlin/delegate.md %}