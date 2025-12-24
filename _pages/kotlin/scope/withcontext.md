---
title: withContext 切換執行緒
date: 2025-10-31
keywords: kotlin, withContext
---
Prerequisites:

- [切換執行緒][1]

## withContext與launch差別
- launch：啟動一個「新協程」，不阻塞當前執行
- withContext：不會啟動「新協程」，沿用原本的協程，切換執行緒，暫停當前協程直到完成。

withContext不會再建立協程，沿用原來的協程，但會改變執行緒。<br>

withContext是順序執行。<br>

```
launch:                 withContext:
┌─────────┐              ┌─────────┐
│ 任務A   │              │  任務A  │
└───┬─────┘              └────┬────┘
    ↓                         ↓
┌───▼─────┐              ┌────▼────┐
│ 同時執行 │              │  等待   │
│ 任務B   │              │  任務B  │
└─────────┘              └────┬────┘
                               ↓
                          ┌────▼────┐
                          │ 繼續執行│
                          │  任務A  │
                          └─────────┘
```

### withContext執行順序:
- 會暫停當前協程
- 等待區塊內程式執行完成
- 自動切回原本的執行緒
- 返回結果

{% highlight kotlin linenos %}
suspend fun fetchUserData(): User {
    // 在 IO 執行緒執行網路請求
    val userData = withContext(Dispatchers.IO) {
        // 這個區塊在 IO 執行緒執行
        apiService.getUserData()
    }
    // 自動回到原本的執行緒
    return userData
}
{% endhighlight %}

### launch執行順序:
{% highlight kotlin linenos %}
fun processData() {
    viewModelScope.launch {
        // 主執行緒執行
        showLoading()
        
        // 啟動新的協程做網路請求（不阻塞）
        val job = launch(Dispatchers.IO) {
            // 在背景執行
            val data = apiService.getData()
            
            // 切回主執行緒更新 UI
            withContext(Dispatchers.Main) {
                updateUI(data)
            }
        }
    }
}
{% endhighlight %}

不會阻塞當前執行

不等待完成

返回 Job 物件（可用來取消）

## Dispatcher
- Dispatchers.IO
- Dispatchers.Default
- Dispatchers.Main

|Dispatcher|  用途|  執行緒數量| 範例|
|:------|:-----|:------|:-----|:---------------|
|Main|  UI 更新| 1| textView.text = "..."|
|IO|  網路/檔案| 64+| retrofit.get()、file.read()|
|Default| CPU 計算|  CPU 核心數| list.sort()、圖片壓縮|
|自訂|  特殊需求|  自訂|  資料庫操作、特殊任務|

{% highlight kotlin linenos %}
suspend fun showDispatchers() {
    // Main - UI 操作
    withContext(Dispatchers.Main) {
        println("UI 執行緒: ${Thread.currentThread().name}")
        // 更新 UI
    }
    
    // IO - 網路/檔案操作
    withContext(Dispatchers.IO) {
        println("IO 執行緒: ${Thread.currentThread().name}")
        // 讀取檔案、網路請求
    }
    
    // Default - CPU 密集計算
    withContext(Dispatchers.Default) {
        println("Default 執行緒: ${Thread.currentThread().name}")
        // 排序、計算、圖片處理
    }
    
    // 自訂執行緒
    withContext(newSingleThreadContext("MyThread")) {
        println("自訂執行緒: ${Thread.currentThread().name}")
    }
}
{% endhighlight %}

## withContext 的返回值
withContext 返回它區塊內的最後一行表達式的值

|情境|  返回值類型| 範例|
|:-----|:------|:------|
|簡單值| 基本類型|  String、Int、Boolean|
|物件|  自訂類別|  User、Post|
|集合|  集合類型|  List<String>、Map<Int, User>|
|可能空值|  可空類型|  User?、String?|
|成功/失敗| sealed class|  Result<T>|
|多個值| Pair/Triple Pair|<String, Int>|
|沒有值| Unit|  Unit|

- withContext 總是有返回值 - 區塊內的最後一行
- 返回類型自動推斷 - 根據最後一行的類型
- 可以是任何類型 - 基本類型、物件、集合、null 等
- 與 launch 的關鍵差別 - launch 沒有返回值
- 實際應用 - 網路請求、資料庫操作、檔案讀寫等需要結果的操作

### 返回String
{% highlight kotlin linenos %}
val result: String = withContext(Dispatchers.IO) {
    // 做一些工作...
    "Hello World"  // ← 這就是返回值
}

println(result)  // 輸出: Hello World
{% endhighlight %}


{% highlight kotlin linenos %}
suspend fun getUserName(): String = withContext(Dispatchers.IO) {
    val user = api.getUser()
    user.name  // ← 返回 String
}

// 使用
val name: String = getUserName()
println("用戶名: $name")
{% endhighlight %}

### 返回Int
{% highlight kotlin linenos %}
suspend fun calculateSum(): Int = withContext(Dispatchers.Default) {
    val a = 10
    val b = 20
    a + b  // ← 返回 Int
}

val sum: Int = calculateSum()  // sum = 30
{% endhighlight %}

### 返回物件
{% highlight kotlin linenos %}
data class User(val id: Int, val name: String)

suspend fun fetchUser(): User = withContext(Dispatchers.IO) {
    // 從 API 取得資料
    val response = api.getUserData()
    User(response.id, response.name)  // ← 返回 User 物件
}

val user: User = fetchUser()
{% endhighlight %}

### 返回多類型
{% highlight kotlin linenos %}
// 返回 Boolean
suspend fun checkNetwork(): Boolean = withContext(Dispatchers.IO) {
    return@withContext try {
        api.ping()
        true   // ← 返回 最後一行
    } catch (e: Exception) {
        false  // ← 返回 
    }
}

// 返回 List
suspend fun getItems(): List<String> = withContext(Dispatchers.IO) {
    listOf("A", "B", "C")  // ← 返回 List
}

// 返回 Unit（不返回有用值）
suspend fun logEvent(): Unit = withContext(Dispatchers.IO) {
    println("事件記錄")  // ← 返回 Unit
    // 或寫 return@withContext Unit
}
{% endhighlight %}

### 與 launch比較
{% highlight kotlin linenos %}
// withContext：有返回值
suspend fun getData(): String = withContext(Dispatchers.IO) {
    "資料"  // ← 可以返回值
}

// launch：沒有返回值
fun doTask() {
    viewModelScope.launch(Dispatchers.IO) {
        "資料"  // ← 這個值無法取得！
        // launch 不接受返回值
    }
}
{% endhighlight %}

## 協程作用域
因為withContext是suspend 函式，所以要在協程作用域中，才能使用withContext()函式。<br>

Junit的coroutine scope(協程作用域)是runBlocking，只有在runBlocking協程作用域下，就能呼叫suspend()函式。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine13() = runBlocking {
  	// 切換至IO執行緒
    withContext(Dispatchers.IO) {
       // 程式碼
    }
    println(result2)
  }
{% endhighlight %}

### 使用方式1
withContext是suspend函式，要放在suspend函式中。<br>
{% highlight kotlin linenos %}
  suspend fun funName() {
    withContext(Dispatchers.IO) {
      delay(1000)
      println("test")
    }
  }

  @Test
  fun coroutine14() = runBlocking {
    funName()
  }
{% endhighlight %}

### 使用方式2
withContext 放在 「等號 =」右邊，傳回值為Unit。 
{% highlight kotlin linenos %}
  suspend fun funName() = withContext(Dispatchers.IO) {
      delay(1000)
      println("test")
  }

  @Test
  fun coroutine14() = runBlocking {
    funName()
  }
{% endhighlight %}
```
test
``` 
### 使用方式3
使用withContext
{% highlight kotlin linenos %}
  suspend fun funName() = coroutineScope {
    withContext(Dispatchers.IO) {
      delay(1000)
      println("test")
    }
  }

  @Test
  fun coroutine14() = runBlocking {
    funName()
  }
{% endhighlight %}
```
test
```

### 使用方式4
包在協程作用域中(coroutineScope、CoroutineScope、GlobalScope)。
{% highlight kotlin linenos %}
  @Test
  fun coroutine14() = runBlocking {
    coroutineScope {
      withContext(Dispatchers.IO) {
        delay(1000)
        println("test")
      }
    }
  }
{% endhighlight %}

### 使用方式5
包在協程中(launch、async)。
{% highlight kotlin linenos %}
  @Test
  fun coroutine14() = runBlocking {
    launch {
      withContext(Dispatchers.IO) {
        delay(1000)
        println("test")
      }
    }
    println()
  }
{% endhighlight %}

### 使用方式6
在Activity，不能在Test Case。
{% highlight kotlin linenos %}
GlobalScope.launch (Dispatchers.Main) {
  // 切換至io
  withContext (Dispatchers.IO) {
    delay(1000)
    println("io")
  }
  // 切換回 main
  println("main")
}
{% endhighlight %}

## 順序執行
即便result2的delay秒數才500，但也是先執行完result1，才執行result2。
{% highlight kotlin linenos %}
  @Test
  fun coroutine13() = runBlocking<Unit> {
    val result1 = withContext(Dispatchers.IO) {
      delay(1000)
      "result1"
    }
    println(result1)
    val result2 = withContext(Dispatchers.IO) {
      delay(500)
      "result2"
    }
    println(result2)
  }
{% endhighlight %}
```
result1
result2
```

## 非同步執行
非同步，我自己的記法是多個協程同時執行，不用等待其它協程執行完畢。<br>
使用launch包住withContext，就可以變成非同步。<br>
{% highlight kotlin linenos %}
  @Test
  fun coroutine15() = runBlocking {
    launch {
      val result1 = withContext(Dispatchers.IO) {
        delay(1000)
        "result1"
      }
      println(result1)
    }

    launch {
      val result2 = withContext(Dispatchers.IO) {
        delay(500)
        "result2"
      }
      println(result2)
    }
    println()
  }
{% endhighlight %}
```
result2
result1
```

[1]: {% link _pages/kotlin/context.md %}