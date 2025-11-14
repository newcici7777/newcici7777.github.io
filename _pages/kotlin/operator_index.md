---
title: [] 運算子
date: 2025-11-14
keywords: kotlin, index operator
---
`[]` 的用途就是 用索引（index）去取得或設定值：

a[0] → 取得 index = 0 的值

a[0] = 10 → 設定 index = 0 的值

Kotlin 將這個行為對應到：

```
operator fun get(index: ...)
operator fun set(index: ..., value: ...)
```

{% highlight kotlin linenos %}
class MyMap {
    private val map = mutableMapOf<String,String>()
    operator fun get(key:String) : String {
        return map[key] ?: ""
    }
    operator fun set(key:String, value: String) {
        map[key] = value
    }
}
fun main() {
    val myMap = MyMap()
    myMap["name"] = "alice"
    println("name = ${myMap["name"]}")
    myMap["address"] = "Taiwan"
    println("address = ${myMap["address"]}")
}
{% endhighlight %}
```
name = alice
address = Taiwan
```

## 其它運算子

|運算子|	覆寫方法|	說明|
|:---|:-----|:-----|
|[]|	get/set|	索引|
|in|	contains|	是否包含|
|==|	equals|	比較|
|()|	invoke|	呼叫|
|..|	rangeTo|	區間|