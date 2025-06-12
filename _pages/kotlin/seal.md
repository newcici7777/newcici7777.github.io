---
title: Seal class
date: 2025-06-09
keywords: kotlin, seal class
---
Prerequisites:

- [Java 靜態內部類別][1]

Kotlin提供Seal class，類似Enum的功能，但看了Java原始碼才發現其實是靜態內部類別。

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

### Java原始碼
{% highlight java linenos %}
// NetworkEvent
public abstract class NetworkEvent {
   // 私有建構子
   private NetworkEvent() {
   }

   // 公有建構子
   public NetworkEvent(DefaultConstructorMarker $constructor_marker) {
      this();
   }

   // Load是一個靜態內部類別，且不可以再被繼承final
   // 繼承NetworkEvent
   public static final class Load extends NetworkEvent {
      // 靜態final的Singleton物件
      @NotNull
      public static final Load INSTANCE;

      // 私有建構子
      private Load() {
         // 呼叫父類別的有參數建構子，但傳null進去。
         super((DefaultConstructorMarker)null);
      }

      // 靜態區塊
      static {
         // 類別載入時，就建立Singleton物件
         Load var0 = new Load();
         // var0的記憶體位址 指派 給INSTANCE
         INSTANCE = var0;
      }
   }
}
{% endhighlight %}

## 密封類別的內部類別
{% highlight kotlin linenos %}
sealed class NetworkEvent {
    data class Success(val code: Int): NetworkEvent()
    class Error(val errCode:Int, val msg: String) : NetworkEvent()
}
{% endhighlight %}

Success與Error是NetworkEvent的內部類別，而且繼承:NetworkEvent()

### Java原始碼
#### Success 
因為內部類別Success是Data Class，Data Class本身就會覆寫component、copy、toString、equals等方法，這些方法就先截掉。
{% highlight java linenos %}
public abstract class NetworkEvent {
    .
    .
    .
    // 靜態內部類別，不可被繼承。
   public static final class Success extends NetworkEvent {
      private final int code;

      public final int getCode() {
         return this.code;
      }

      // 建構子
      public Success(int code) {
         super((DefaultConstructorMarker)null);
         this.code = code;
      }
   }
}
{% endhighlight %}
#### Error
{% highlight java linenos %}
public abstract class NetworkEvent {
    .
    .
    .
    // 靜態內部類別，不可被繼承。
   public static final class Error extends NetworkEvent {
      // 私有 不可更改 屬性
      private final int errCode;
      @NotNull
      private final String msg;

      public final int getErrCode() {
         return this.errCode;
      }

      @NotNull
      public final String getMsg() {
         return this.msg;
      }
      
      // 建構子
      public Error(int errCode, @NotNull String msg) {
         Intrinsics.checkNotNullParameter(msg, "msg");
         super((DefaultConstructorMarker)null);
         this.errCode = errCode;
         this.msg = msg;
      }
   }
}
{% endhighlight %}

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

### Java原始碼
判斷的部分。
{% highlight java linenos %}
public static final String checkStatus(@NotNull NetworkEvent status) {
   // 檢查參數是否為null
   Intrinsics.checkNotNullParameter(status, "status");
   // 傳回值
   String var10000;

   // 判斷是不是Load類別
   if (status instanceof NetworkEvent.Load) {
      var10000 = "正在載入中";
   } else if (status instanceof NetworkEvent.Success) { // 判斷是否為Success類別
      var10000 = "成功，code = " + ((NetworkEvent.Success)status).getCode();
   } else {
      // 判斷是否為Error類別
      if (!(status instanceof NetworkEvent.Error)) {
         throw new NoWhenBranchMatchedException();
      }

      var10000 = "失敗, code = " + ((NetworkEvent.Error)status).getErrCode() + " , msg = " + ((NetworkEvent.Error)status).getMsg();
   }
   
   return var10000;
}
{% endhighlight %}

main的部分。
{% highlight java linenos %}
public static final void main() {
   NetworkEvent.Load loading = NetworkEvent.Load.INSTANCE;
   NetworkEvent.Success success = new NetworkEvent.Success(200);
   NetworkEvent.Error error = new NetworkEvent.Error(404, "Page not found");
   String result = checkStatus((NetworkEvent)success);
   System.out.println(result);
   result = checkStatus((NetworkEvent)error);
   System.out.println(result);
   result = checkStatus((NetworkEvent)loading);
   System.out.println(result);
}
{% endhighlight %}


[1]: {% link _pages/java/static_inner.md %}