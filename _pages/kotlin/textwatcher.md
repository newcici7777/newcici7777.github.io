---
title: TextWatcher
date: 2025-11-10
keywords: kotlin, TextWatcher
---
Prerequisites:

- [callbackFlow][1]

傳統的EditText是透過addTextChangedListener來判斷輸入文字改變。
{% highlight kotlin linenos %}
editText.addTextChangedListener(object : TextWatcher {
  override fun afterTextChanged(s: Editable?) {
    // 1個1個的文字輸入完，會呼叫這個
    Log.d(TAG, "afetr change $s")
  }

  override fun beforeTextChanged(
    s: CharSequence?,
    p1: Int,
    p2: Int,
    p3: Int
  ) {
    Log.d(TAG, "before change $s")
  }

  override fun onTextChanged(
    s: CharSequence?,
    p1: Int,
    p2: Int,
    p3: Int
  ) {
    Log.d(TAG, " changing $s")
  }

})
{% endhighlight %}

如果要使用協程，要使用callbackFlow，使用trySend()通知輸入內容已經改變，awaitClose()關閉資源，collect是接收改變的文字。
{% highlight kotlin linenos %}
import android.os.Bundle
import android.util.Log
import android.widget.EditText
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.core.widget.doAfterTextChanged
import androidx.lifecycle.lifecycleScope
import com.example.coroutine.R
import kotlinx.coroutines.channels.awaitClose
import kotlinx.coroutines.flow.callbackFlow
import kotlinx.coroutines.flow.Flow
import kotlinx.coroutines.launch

class TextWatchActivity : AppCompatActivity() {
  private val TAG = "TextWatcher"
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_textwatch)
    val text_watcher = findViewById<EditText>(R.id.text_watcher)
    lifecycleScope.launch {
      text_watcher.textWatcherFlow()
        .collect { text ->
          Log.d(TAG, "afetr change $text")
        }
    }
  }
  private fun TextView.textWatcherFlow(): Flow<String> = callbackFlow {
    val textWatcher = doAfterTextChanged { editableText ->
      trySend(editableText?.toString() ?: "")
    }
    awaitClose {
      removeTextChangedListener(textWatcher)
    }
  }
}
{% endhighlight %}
```
2025-11-10 15:00:45.647  3823-3823  TextWatcher             com.example.coroutine                D  afetr change a
2025-11-10 15:00:46.099  3823-3823  TextWatcher             com.example.coroutine                D  afetr change aa
2025-11-10 15:00:49.611  3823-3823  TextWatcher             com.example.coroutine                D  afetr change aad
2025-11-10 15:00:49.627  3823-3823  TextWatcher             com.example.coroutine                D  afetr change aadf
````

[1]: {% link _pages/kotlin/callbackflow.md %}
