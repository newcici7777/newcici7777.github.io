---
title: Global Scope
date: 2025-12-24
keywords: Android, Kotlin, coroutines, Global Scope
---
## 生命周期
- GlobalScope App結束才會Destoryed。
- lifecycleScope Activity結束才會Destoryed
- viewModelScope 在ViewModel使用，Activity結束才會Destoryed

GlobalScope App結束才會Destoryed，建議別使用，避免記憶體洩露。

{% highlight kotlin linenos %}
GlobalScope.launch {
    // 永不自動取消！
    // App關閉才會停止
    while (true) {
        delay(1000)
        println("還在跑...")  // 記憶體洩漏！
    }
}
{% endhighlight %}

## GlobalScope與lifecycleScope
執行下面的程式，會發生即便Activity結束，無限迴圈仍在執行，不會隨著Activity結束而結束。<br>
{% highlight kotlin linenos %}
class MainActivity10 : AppCompatActivity() {
  val TAG = "MainActivity10"
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)

    submit.setOnClickListener {
      GlobalScope.launch {
      	// 無限迴圈
        while(true) {
          // 每1秒印一次still runing
          delay(1000)
          Log.d(TAG, "still runing")
        }
      }
      // 5秒後轉到其它Activity，Activity10要結束
      GlobalScope.launch {
        delay(5000)
        Intent(this@MainActivity10, MainActivity01::class.java).also {
          startActivity(it)
          finish()
        }
      }
    }
  }
}
{% endhighlight %}

## GlobalScope 與 runBlocking測試
GlobalScope是獨立作用域，加上join，runBlocking才會等待GlobalScope執行完畢。
{% highlight kotlin linenos %}
  @Test
  fun coroutin08() = runBlocking {
    val job = GlobalScope.launch {
      delay(1000)
      println("job1")
    }
    job.join()
  }
{% endhighlight %}
```
job1
```