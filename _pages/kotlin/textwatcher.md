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
    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {
        // 文字改變前呼叫
        Log.d("TextWatcher", "beforeTextChanged: $s")
    }

    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {建立外部類別
        // 文字改變時呼叫
        Log.d("TextWatcher", "onTextChanged: $s")
    }

    override fun afterTextChanged(s: Editable?) {
        // 文字改變後呼叫
        Log.d("TextWatcher", "afterTextChanged: $s")
        
        // 在這裡處理文字改變後的邏輯
        if (s?.length ?: 0 > 10) {
            editText.error = "文字長度不能超過10個字元"
        } else {
            editText.error = null
        }
    }
})
{% endhighlight %}

如果要使用協程，要使用callbackFlow，使用trySend()通知輸入內容已經改變，awaitClose()關閉資源，collect是接收改變的文字。

1. 擴展函數 (Extension Function)
```
fun TextView.textWatcherFlow(): Flow<String>
```
為 TextView 類別添加新方法

可以在任何 TextView 實例上呼叫

2. callbackFlow Builder
```
callbackFlow { 
    // 將回調轉換為 Flow
}
```
用於將回調式 API 轉換為 Flow

3. trySend()
```
trySend(s?.toString() ?: "")
```
向 Flow 發送數據（舊版本用 offer()）

內部使用 trySend() 發送數據

4. awaitClose()
```
awaitClose {
    removeTextChangedListener(textWatcher)
}
```
定義當 Flow 被取消時要執行的清理操作

必須呼叫，否則會編譯錯誤

必須使用 awaitClose() 清理資源

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
