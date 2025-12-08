---
title: TextField
date: 2023-05-03
keywords: Android, Jetpack compose, TextField 
---
Prerequisites:

- [Remember MutableState][1]

明確的導入remember
```
import androidx.compose.runtime.remember
import androidx.compose.runtime.mutableStateOf
```

{% highlight kotlin linenos %}
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

![img]({{site.imgurl}}/compose/textfield3.png)

------------------------------------------
以下是舊文章

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

[1]: {% link _pages/compose/mutablestate.md %}