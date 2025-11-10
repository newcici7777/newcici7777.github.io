---
title: callbackFlow
date: 2025-11-10
keywords: kotlin, callbackFlow
---
callbackFlow 是 Kotlin 協程 (coroutine) 中的一個非常實用的工具，用來把「回呼 (callback) 式 API」轉換成 Flow。

callbackFlow 讓你可以「把 callback 事件」包裝成一個 Flow 來收集。

有時候你要使用的 API 並不是掛在協程或 Flow 上的，而是「傳統 callback 型」：

fun listenToLocationUpdates(listener: (Location) -> Unit)


這種 API 無法用 suspend 或 Flow 直接收集到值。
這時你就可以用 callbackFlow 來把它「包裝」成 Flow。

使用時機

使用 callbackFlow 的時機主要是：

你要把 callback 型 API 轉換成 Flow。

你想用 Flow 的優點（例如：collect、map、filter、timeout 等操作）。

你需要在協程中使用「連續資料流」但來源不是協程（例如 Android 的 Listener、Firebase 監聽、WebSocket 等）。

注意事項

trySend() 會嘗試發送資料給 collector。若 Flow 滿了會失敗，不會掛起。

若要保證發送成功（可掛起），可用 send()。

awaitClose { } 是用來在 Flow 被取消或結束時清理 callback（如移除 listener）。

{% highlight kotlin linenos %}
class LocationService {
    fun startListening(callback: (String) -> Unit) {
        // 模擬持續回傳位置
        Thread {
            repeat(3) {
                Thread.sleep(1000)
                callback("Location $it")
            }
        }.start()
    }

    fun stopListening() {
        println("Stopped listening.")
    }
}
{% endhighlight %}

{% highlight kotlin linenos %}
fun locationFlow() = callbackFlow {
    val service = LocationService()

    service.startListening { location ->
        trySend(location) // 傳送資料到 flow
    }

    // onClose 時清理資源
    awaitClose {
        service.stopListening()
    }
}
{% endhighlight %}