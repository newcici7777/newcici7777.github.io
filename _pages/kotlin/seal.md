---
title: Seal class
date: 2025-06-09
keywords: kotlin, seal class
---
Kotlin提供Seal class取代Enum。

建立一個密封類別，有三個屬性，全部繼承密封類別。
{% highlight kotlin linenos %}
sealed class NetworkEvent {
    object Load: NetworkEvent()
    data class Success(val code: Int): NetworkEvent()
    class Error(val errCode:Int, val msg: String) : NetworkEvent()
}
{% endhighlight %}

## object
建立一個Singleton物件
{% highlight kotlin linenos %}
object Load: NetworkEvent()
{% endhighlight %}

Kotiln在私下已經建立了Singleton物件。
{% highlight kotlin linenos %}
class Load private constructor(): NetworkEvent() {
    companion object {
        val INSTANCE = Load()
    }
}
{% endhighlight %}

使用Seal Class類別名.屬性，就會自動幫Load屬性，建立好Singleton物件。
{% highlight kotlin linenos %}
val loading = NetworkEvent.Load
{% endhighlight %}

不能這樣寫。
{% highlight kotlin linenos %}
val loading = NetworkEvent.Load()
{% endhighlight %}

## 密封類別的巢狀類別
{% highlight kotlin linenos %}
sealed class NetworkEvent {
    data class Success(val code: Int): NetworkEvent()
    class Error(val errCode:Int, val msg: String) : NetworkEvent()
}
{% endhighlight %}

Success與Error是NetworkEvent的巢狀類別，而且繼承:NetworkEvent()

## 判斷與使用
使用when作為判斷
{% highlight kotlin linenos %}
fun checkStatus(status: NetworkEvent): String {
    return when(status) {
        is NetworkEvent.Load -> "正在載入中"
        is NetworkEvent.Success -> "成功，code = ${status.code}"
        is NetworkEvent.Error -> "失敗, code = ${status.errCode} , msg = ${status.msg}"
    }
}
{% endhighlight %}

測試
{% highlight kotlin linenos %}
fun main() {
    val loading = NetworkEvent.Load
    val success =  NetworkEvent.Success(200)
    val error = NetworkEvent.Error(404, "Page not found")

    var result = checkStatus(success)
    println(result)
    result = checkStatus(error)
    println(result)
    result = checkStatus(loading)
    println(result)
}
{% endhighlight %}
```
成功，code = 200
失敗, code = 404 , msg = Page not found
正在載入中
```