---
title: ViewBinding
date: 2025-10-15
keywords: kotlin, ViewBinding
---
## build.gradle設置
app下的build.gradle設置以下內容，設置完後一定要<span class="markline">同步Sync</span>。<br>
{% highlight groovy linenos %}
android {
    ...

    buildFeatures {
        viewBinding = true
    }
}
{% endhighlight %}

import
{% highlight kotlin linenos %}
import com.example.coroutine.databinding.ActivityMainBinding
{% endhighlight %}

## viewbinding步驟

### binding變數
lateinit是延後初始化，就不用給binding先設值。
{% highlight kotlin linenos %}
private lateinit var binding : ActivityMainBinding
{% endhighlight %}

### inflate
綁定xml
{% highlight kotlin linenos %}
binding = ActivityMainBinding.inflate(layoutInflater)
{% endhighlight %}

### 設定root
找到xml第一個元素
{% highlight kotlin linenos %}
setContentView(binding.root)
{% endhighlight %}

androidx.constraintlayout.widget.ConstraintLayout就是root。<br>
{% highlight css linenos %}
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".activity.MainActivity07">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Add"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
{% endhighlight %}

### 使用元件
{% highlight kotlin linenos %}
binding.textView.text = "Test"
{% endhighlight %}

### 完整程式碼
{% highlight kotlin linenos %}
class MainActivity13 : AppCompatActivity() {
  // 延後初始化
  private lateinit var binding : ActivityMainBinding
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
    val submit = binding.button
    val textv = binding.textView

    submit.setOnClickListener {
      textv.text = "Test, Test"
    }
  }
}
{% endhighlight %}