---
title: TextField
date: 2023-05-03
keywords: Android, Jetpack compose, TextField 
---
明確的導入remember
```
// 或是更明確的導入
import androidx.compose.runtime.remember
import androidx.compose.runtime.mutableStateOf
```

{% highlight kotlin linenos %}
@Composable
fun testStatus() {
  // 輸入框 - 會自動上移避開軟鍵盤
  // 1. 創建一個可記住的狀態
  var text by remember { mutableStateOf("") }

  // 2. 將狀態傳遞給 TextField
  TextField(
    value = text,  // 使用狀態值
    onValueChange = { newText ->
      text = newText  // 更新狀態
    },
    label = { Text("輸入文字") },
    modifier = Modifier
      .fillMaxWidth()
      .padding(16.dp)
  )
}
{% endhighlight %}

以下是舊文章
------------------------------------------

![img]({{site.imgurl}}/compose/textfield1.png)
![img]({{site.imgurl}}/compose/textfield2.png)
{% highlight kotlin linenos %}
@Composable
fun TextFieldSample() {
    //使用by remember才會記憶value
    var value by remember {
        mutableStateOf("")
    }
    TextField(
        value = value,
        onValueChange = {
            //把輸入的值放在value變數
            value = it
        },
        label = {//標籤
            Text("name")
        },
        placeholder = {
            Text(text = "請輸入")
        },
        leadingIcon = {//前置圖片
            Icon(imageVector = Icons.Default.AccountBox, contentDescription = null)
        },
        keyboardActions = KeyboardActions(onDone = {
            //處理按下完成
        }),
        singleLine = true,//單行
        keyboardOptions = KeyboardOptions(
            imeAction = ImeAction.Done,//回車鍵有完成的按鈕
            keyboardType = KeyboardType.Number//數字
        )
    )
}
@Preview
@Composable
fun TextFieldSamplePreview() {
    TextFieldSample()
}
{% endhighlight %}