---
title: Icon
date: 2023-05-03
keywords: Android, Jetpack compose, Switch/Image/Icon 
---
## checked
checked變數設為false 
可使用來把變數變相反 
checked = !checked  
也可以使用it來取得switch是true 或false 
{% highlight kotlin linenos %}
@Composable
fun SwitchSample() {
    var checked by remember{ mutableStateOf(false)}
    Switch(checked = checked, onCheckedChange = {
        //checked = !checked 
        checked = it
    })
}
{% endhighlight %}

## ICON使用官方圖庫
build.gradle(Module :app)中 
```
dependencies {
implementation "androidx.compose.material:material-icons-extended:1.4.2"
}
```
### Icon tint
```
Icon(imageVector = Icons.Default.Translate, contentDescription = null, tint = Color.Red)
```
可換成 `Icons.Default.AccountBox`  
參考以下網址
[https://fonts.google.com/icons?icon.query=Account](https://fonts.google.com/icons?icon.query=Account)

tint = Color.Red背景顏色紅色  
![img]({{site.imgurl}}/compose/compose_icon1.png)
{% highlight kotlin linenos %}
@Composable
fun IconSample() {
    Icon(imageVector = Icons.Default.Translate, contentDescription = null, tint = Color.Red)
}
{% endhighlight %}

## ICON使用painterResource
![img]({{site.imgurl}}/compose/compose_icon2.png)
![img]({{site.imgurl}}/compose/compose_icon3.png)
{% highlight kotlin linenos %}
@Composable
fun IconSample() {
    Icon(painter = painterResource(id = ic_android_black_24dp), contentDescription = null)
}
{% endhighlight %}

## ImageView
{% highlight kotlin linenos %}
Image(
    painter = painterResource(id = R.drawable.ic_android_black_24dp),
    contentDescription = null,
    modifier = Modifier.size(50.dp),//大小
    colorFilter = ColorFilter.tint(Color.Red, blendMode = BlendMode.Color)//紅色背景
)
{% endhighlight %}
![img]({{site.imgurl}}/compose/compose_icon4.png)