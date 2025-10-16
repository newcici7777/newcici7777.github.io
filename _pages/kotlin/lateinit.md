---
title: lateinit
date: 2025-10-15
keywords: kotlin, lateinit
---
## 語法
不用一開始就要初始化變數，使用的時候，才初始化。<br>

宣告一個非空（non-null）的變數，但不立即賦值，而是在之後的某個時間點再初始化。<br>

使用時機:<br>
用 lateinit：確定變數<span class="markline">一定會被初始化</span>，且只需要初始化一次

語法
```
private lateinit var 變數名: 類型
```

## Android 的 ViewBinding
原本這樣寫會出錯，強迫一定要初始化
{% highlight kotlin linenos %}
private var binding : ActivityMainBinding
{% endhighlight %}

在var前面加上lateinit延後初始化，就不用一開始就一定要初始化。<br>
{% highlight kotlin linenos %}
private lateinit var binding : ActivityMainBinding
{% endhighlight %}

沒加lateinit，又不設預設值，會編譯錯誤。
{% highlight kotlin linenos %}
class MyActivity : AppCompatActivity() {
    // 編譯錯誤：必須在構造函式中初始化
    private var binding: ActivityMainBinding
    
    // 但這樣寫很醜，要用 null 和 !! 
    private var binding: ActivityMainBinding? = null
}
{% endhighlight %}

完整初始化程式碼
{% highlight kotlin linenos %}
class MainActivity13 : AppCompatActivity() {
  // 延後初始化
  private lateinit var binding : ActivityMainBinding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // 在這初始化
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
  }
}
{% endhighlight %}

## 類別成員變數
判斷是否有初始值
```
::變數.isInitialized
```

{% highlight kotlin linenos %}
class MyClass {
    lateinit var retrofit: String
    fun init() {
        retrofit = "retrofit is ready!"
    }

    fun use() {
        if (::retrofit.isInitialized) {
            println(retrofit)
        }
    }
}

fun main() {
    val myclass = MyClass()
    myclass.init()
    myclass.use()
}
{% endhighlight %}
```
retrofit is ready!
```

## 不能用 lateinit 的情況
基本類型不能用lateinit，會編譯錯誤。<br>
{% highlight kotlin linenos %}
    lateinit var count: Int
    lateinit var price: Double
    lateinit var isInit: Boolean
{% endhighlight %}

可空類型不能用lateinit
{% highlight kotlin linenos %}
lateinit var name: String?
{% endhighlight %}

建構子也不能用lateinit
{% highlight kotlin linenos %}
class MyClass(lateinit var name: String) {
}
{% endhighlight %}

## 可空類型 vs lateinit
可空類型，使用前要用 ? 或 .let{} 判斷是否為null。
{% highlight kotlin linenos %}
class MyClass2() {
    var name: String? = null
    fun init() {
        name = "Bill"
    }

    fun use() {
        println(name?.length ?: 0)
    }
}
{% endhighlight %}

lateinit，使用時不用加 ?
{% highlight kotlin linenos %}
class MyClass2() {
    lateinit var name: String
    fun init() {
        name = "Bill"
    }

    fun use() {
        println(name.length)
    }
}
{% endhighlight %}