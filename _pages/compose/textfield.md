---
title: TextField
date: 2023-05-03
keywords: Android, Jetpack compose, TextField 
---
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