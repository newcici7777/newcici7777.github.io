---
title: MutableStateFlow StateFlow
date: 2025-10-14
keywords: kotlin, StateFlow, MutableStateFlow
---
## 唯讀 與 可存取
StateFlow是<span class="markline">唯讀</span>，唯讀代表不可以修改，只能讀取。<br>

MutableStateFlow是可讀<span class="markline">可寫</span>，可以存取。<br>

MutableStateFlow僅能在 ViewModel 中<span class="markline">修改</span>變數。<br>

StateFlow提供給Activity來<span class="markline">讀取</span>ViewModel的MutableStateFlow變數。<br>

MutableStateFlow<span class="markline">本身就有emit()</span>發射的功能，不需要自己寫。<br>

所以當MutableStateFlow(可讀可寫)變數有任何修改，就自動發射emit()。<br>

Activity，使用StateFlow(唯讀)的collect()，監控是否有變更，有變更就要接收變更的值。<br>

## MutableStateFlow.value
value代表變數值，本身就有setValue()與getValue()的功能。<br>

修改
```
MutableStateFlow變數.value = MutableStateFlow變數.value + 1
_number.value++
```

## ViewModel與MutableStateFlow
MutableStateFlow的變數都是以底線_開頭。<br>

變數不是null
```
val _變數 = MutableStateFlow<變數類型>(初始值)
val _number = MutableStateFlow<Int>(0)
```

變數可以是null
```
val _變數 = MutableStateFlow<變數類型>(初始值)
val _str = MutableStateFlow<String?>(null)
```

## ViewModel與StateFlow
StateFlow的變數不是底線_開頭。<br>

變數不是null
```
val 變數: StateFlow<變數類型> = _MutableStateFlow變數
val number: StateFlow<Int> = _number
```

## 完整程式碼
NumberViewModel
{% highlight kotlin linenos %}
import androidx.lifecycle.ViewModel
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow

class NumberViewModel : ViewModel() {
  val _number = MutableStateFlow<Int>(0)
  val number: StateFlow<Int> = _number
  fun increase() {
    _number.value++
  }
}
{% endhighlight %}

MainActivity08是使用StateFlow來做collect()監控並接收變動的資料。<br>
lifecycleScope是Activity的協程，生命周期與Activity一樣，Activity Destoryed的時候，協程占用的記憶體也會被記憶體回收，不會有殘留的記憶體沒被清除。<br>

collect()監控並接收變更資料，是suspend()暫停函式，必須在協程的區域中才能被呼叫。<br>

value是變更的資料。<br>

MainActivity08
{% highlight kotlin linenos %}
class MainActivity08 : AppCompatActivity() {
  private val viewModel by viewModels<NumberViewModel>()
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    lifecycleScope.launch {
      viewModel.number.collect { value ->
        textv.text = value.toString()
      }
    }
    submit.setOnClickListener {
      viewModel.increase()
    }
  }
}
{% endhighlight %}