---
title: LiveData
date: 2025-10-15
keywords: kotlin, MutableLiveData, LiveData
---
已棄用。<br>

下方程式碼與stateflow大部分一模一樣。<br>

## 唯讀 與 可存取
LiveData是<span class="markline">唯讀</span>，唯讀代表不可以修改，只能讀取。<br>

MutableLiveData是可讀<span class="markline">可寫</span>，可以存取。<br>

MutableLiveData僅能在 ViewModel 中<span class="markline">修改</span>變數。<br>

LiveData提供給Activity來<span class="markline">讀取</span>ViewModel的MutableLiveData變數。<br>

LiveData.observe()為<span class="markline">觀察者</span>，觀察MutableLiveData的資料是否有變更。

所以當MutableLiveData(可讀可寫)變數有任何修改，LiveData都會觀察到。<br>

Activity，使用LiveData(唯讀)的observe()，監控是否有變更，有變更就要接收變更的值。<br>

## LifecycleOwner
Activity本身就有實作LifecycleOwner，Activity就是LifecycleOwner。

## LiveData.observe()
第一個參數代入LifecycleOwner，第二個參數則是監控的資料。注意！不是it.value，it本身就是LiveData.value
```
LiveData.observe(LifecycleOwner) {
	it
}
```

## postValue
MutableLiveData本身有getValue與setValue的功能。<br>
postValue()是使用在Thread、協程中。<br>
setValue()是使用在main Thread。<br>

{% highlight kotlin linenos %}
public class MutableLiveData<T> extends LiveData<T> {

    /**
     * Creates a MutableLiveData initialized with the given {@code value}.
     *
     * @param value initial value
     */
    public MutableLiveData(T value) {
        super(value);
    }

    /**
     * Creates a MutableLiveData with no value assigned to it.
     */
    public MutableLiveData() {
        super();
    }

    @Override
    public void postValue(T value) {
        super.postValue(value);
    }

    @Override
    public void setValue(T value) {
        super.setValue(value);
    }
}
{% endhighlight %}

以下是LiveNumberModel，在ViewModel修改用MutableLiveData，變數_number用底線開頭。<br>
Activity使用的是LiveData，僅有唯讀功能，變數number沒有底線開頭。<br>
increase()方法用於Main Thread，更新畫面使用。<br>
increaseThread()方法，使用的是postValue()，在IO Thread、非Main Thread中使用此方法。<br>
{% highlight kotlin linenos %}
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

class LiveNumberModel : ViewModel() {
  private val _number = MutableLiveData<Int>(0)
  val number: LiveData<Int> = _number
  fun increase() {
    _number.value++
  }
  fun increaseThread() {
    var temp = _number.value + 1
    _number.postValue(temp)
  }
}
{% endhighlight %}

在Activity使用IO協程，呼叫postValue()。<br>
如果在IO協程中，呼叫increase()，App會閃退。<br>

number是唯讀的LiveData，observe()是觀察者，this是Activity，Activity本身就是LifecycleOwner。<br>
it是LiveData.value。<br>
{% highlight kotlin linenos %}
viewModel.number.observe(this) {
    textv.text = it.toString()
}
{% endhighlight %}

{% highlight kotlin linenos %}
class MainActivity12 : AppCompatActivity() {
  private val viewModel by viewModels<LiveNumberModel>()
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    viewModel.number.observe(this) {
      textv.text = it.toString()
    }
    submit.setOnClickListener {
      GlobalScope.launch(Dispatchers.IO) {
        viewModel.increaseThread()
        //viewModel.increase()
      }
    }
  }
}
{% endhighlight %}

## LiveData與retrofit
請先查看retrofit文章，再往下繼續看下去。<br>

### LiveViewModel
{% highlight kotlin linenos %}
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import com.example.coroutine.api.ArticleList
import androidx.lifecycle.viewModelScope
import com.example.coroutine.api.ArticleApi
import kotlinx.coroutines.launch

class LiveViewModel : ViewModel() {
  private val _articleList = MutableLiveData<ArticleList>()
  val articleList: LiveData<ArticleList> = _articleList
  fun getArticleList() {
    viewModelScope.launch {
      val result = ArticleApi.retrofit.articleList()
      _articleList.value = result
    }
  }
}
{% endhighlight %}

### Activity使用Livedata
{% highlight kotlin linenos %}
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import com.example.coroutine.R
import com.example.coroutine.model.LiveViewModel


class MainActivity11 : AppCompatActivity() {
  private val viewModel by viewModels<LiveViewModel>()
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    viewModel.articleList.observe(this) {
      textv.text = it.toString()
    }
    submit.setOnClickListener {
      viewModel.getArticleList()
    }
  }
}
{% endhighlight %}