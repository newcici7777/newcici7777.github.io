---
title: ViewModel
date: 2025-10-14
keywords: kotlin, viewModel
---
## 沒有ViewModel
以下的程式碼，會因為畫面旋轉，重繪畫面，導致資料變成0。

MainActivity08
{% highlight kotlin linenos %}
class MainActivity08 : AppCompatActivity() {
  var number = 0
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)
    submit.setOnClickListener {
      number++
      textv.text = number.toString()
    }
  }
}
{% endhighlight %}

xml
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

## ViewModel
viewModel可以保存資料，不會因為畫面旋轉(重繪)，而導致資料變成0。<br>

NumberViewModel
{% highlight kotlin linenos %}
import androidx.lifecycle.ViewModel

class NumberViewModel : ViewModel() {
  var number = 0
  fun increase() {
    number++
  }
}
{% endhighlight %}

MainActivity08
{% highlight kotlin linenos %}
class MainActivity08 : AppCompatActivity() {
  private val viewModel by viewModels<NumberViewModel>()
  var number = 0
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    val submit = findViewById<Button>(R.id.button)
    val textv = findViewById<TextView>(R.id.textView)

    submit.setOnClickListener {
      viewModel.increase()
      textv.text = viewModel.number.toString()
    }
  }
}
{% endhighlight %}

