---
title: Btn
date: 2023-05-03
keywords: Android, Jetpack compose, Btn
---
![img]({{site.imgurl}}/compose/compose_btn1.png)
{% highlight kotlin linenos %}
@Composable
fun ButtonSample() {
    Button(onClick = {
        Log.d("====", "click button")
    }, colors = ButtonDefaults.buttonColors(Color.Red)){//紅色
        Text(text = "click")//字
    }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/compose_btn2.png)
{% highlight kotlin linenos %}
@Composable
fun ButtonSample() {
    //文字Button
    TextButton(onClick = { /*TODO*/ }) {
        Text(text = "test")
    }
}
{% endhighlight %}

![img]({{site.imgurl}}/compose/compose_btn3.png)
{% highlight kotlin linenos %}
@Composable
fun ButtonSample() {
    OutlinedButton(onClick = { /*TODO*/ }) {
        Text("test")
    }
}
{% endhighlight %}