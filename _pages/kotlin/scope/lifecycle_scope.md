---
title: lifecycleScope
date: 2025-10-14
keywords: Android, Kotlin, coroutines, lifecycleScope, GlobalScope
---
## 生命周期
- GlobalScope App結束才會Destoryed。
- lifecycleScope Activity或Fragment結束才會Destoryed

## GlobalScope與lifecycleScope
執行下面的程式，會發即便Activity結束，無限迴圈仍在執行，不會隨著Activity結束而結束。<br>
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

改為lifecycleScope，無限迴圈就會隨著Activity結束，而不會再無限執行。<br>
只會執行五次。<br>
{% highlight kotlin linenos %}
    submit.setOnClickListener {
      lifecycleScope.launch {
        while(true) {
          delay(1000)
          Log.d(TAG, "still runing")
        }
      }
      GlobalScope.launch {
        delay(5000)
        Intent(this@MainActivity10, MainActivity01::class.java).also {
          startActivity(it)
          finish()
        }
      }
    }
{% endhighlight %}
```
2025-10-14 15:43:55.395 12996-12996 MainActivity10          com.example.coroutine                D  still runing
2025-10-14 15:43:56.398 12996-12996 MainActivity10          com.example.coroutine                D  still runing
2025-10-14 15:43:57.401 12996-12996 MainActivity10          com.example.coroutine                D  still runing
2025-10-14 15:43:58.404 12996-12996 MainActivity10          com.example.coroutine                D  still runing
2025-10-14 15:43:59.408 12996-12996 MainActivity10          com.example.coroutine                D  still runing
```