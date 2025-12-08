---
title: Remember MutableState
date: 2023-05-03
keywords: Android, Jetpack compose, mutableState, remeber
---
## Remember 記憶
remember的作用是在記憶體儲存變數。<br>

## MutableState 狀態
MutableState用來儲存變數，一個Composable函式可以有多個remember，每一個remember對映一個MutableState物件，每一個MutableState儲存一個變數。<br>

MutableState對映的變數被「更改」，MutableState就會通知「使用」此變數的UI元件(例:TextField)重繪UI介面。<br>

![img]({{site.imgurl}}/compose/mutablestate1.png)<br>

## 語法
儲存變數到記憶體中。
```
var 變數 by remember { mutableStateOf("預設值") }
var text by remember { mutableStateOf("") }
var num by remember { mutableStateOf(0) }
```

![img]({{site.imgurl}}/compose/textfield3.png)<br>

{% highlight kotlin linenos %}
import androidx.compose.runtime.remember
import androidx.compose.runtime.mutableStateOf

class MainActivity : ComponentActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    enableEdgeToEdge()
    setContent {
      testUI()
    }
  }
}

@Composable
fun testUI() {
  // 1. 儲存text變數在記憶體
  var text by remember { mutableStateOf("") }

  // 2. TextField使用text變數
  TextField(
    value = text,  // 使用狀態值
    onValueChange = { newText ->
      text = newText  // mutableState監控text變數被修改，通知TextField 重繪UI
    },
    modifier = Modifier
      .fillMaxWidth()
      .padding(top = 30.dp)
  )
}
{% endhighlight %}

----------------------------
以下是舊文章。

如果只使用var count = 1; 變數再重新繪製手機介面時就又變成1，所以要使用 by remeber

以下count = mutable類別，要使用count.value讀取
{% highlight kotlin linenos %}
var count = remember {
        mutableStateOf(1)
    }
Text(text = "xx${count.value}xxx", Modifier.clickable {
        count.value++
        Log.d("xxxx","I comin")
    } )
{% endhighlight %}

以下count使用by，count仍是int類別，就不用count.value
{% highlight kotlin linenos %}
    var count by remember {
        mutableStateOf(1)
    }
{% endhighlight %}

{% highlight kotlin linenos %}
@Composable
fun StateSample() {
    var count by remember {
        mutableStateOf(1)
    }
    Log.d("xxxx","outter${count}")
    Text(text = "xx${count}xxx", Modifier.clickable {
        count++
        Log.d("xxxx","I comin")
    } )
}
{% endhighlight %}