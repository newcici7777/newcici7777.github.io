---
title: Padding Margin EdgeInsets
date: 2026-04-01
keywords: flutter, widget, Padding, Margin
---
Container中padding, margin 屬性是EdgeInsets類別。<br>

## EdgeInsets

|方法|說明|
|:----|:-------------|
|all|統一設定上下左右間隔|
|only|個別設定上下左右間隔|
|symmetric|horizontal左右二邊間隔大小,vertical上下間隔大小|

### EdgeInsets.all(double value)

![img]({{site.imgurl}}/flutter/padding1.png)<br>

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
                padding: EdgeInsets.all(20),
                width: double.infinity,
                height: double.infinity,
                color: Colors.red,
                child: Container(
                  alignment: Alignment.center,
                  width: double.infinity,
                  height: double.infinity,
                  color: Colors.blue,
                ))));
  }
}

{% endhighlight %}

### EdgeInsets.only(top,left,right,bottom)
可以只設定top
```
EdgeInsets.only(top: 20)
```

![img]({{site.imgurl}}/flutter/padding4.png)<br>

{% highlight dart linenos %}
class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
                padding:
                    EdgeInsets.only(top: 20, left: 5, right: 30, bottom: 10),
                width: double.infinity,
                height: double.infinity,
                color: Colors.red,
                child: Container(
                  alignment: Alignment.center,
                  width: double.infinity,
                  height: double.infinity,
                  color: Colors.blue,
                ))));
  }
}
{% endhighlight %}

### EdgeInsets.symmetric(horizontal)

![img]({{site.imgurl}}/flutter/padding2.png)<br>

{% highlight dart linenos %}
class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
                padding: EdgeInsets.symmetric(horizontal: 20),
                width: double.infinity,
                height: double.infinity,
                color: Colors.red,
                child: Container(
                  alignment: Alignment.center,
                  width: double.infinity,
                  height: double.infinity,
                  color: Colors.blue,
                ))));
  }
}

{% endhighlight %}

### EdgeInsets.symmetric(vertical)
可以同時設定vertical跟horizontal。<br>
```
EdgeInsets.symmetric(vertical: 10,horizontal: 20)
```

![img]({{site.imgurl}}/flutter/padding3.png)<br>

{% highlight dart linenos %}
class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
                padding: EdgeInsets.symmetric(vertical: 20),
                width: double.infinity,
                height: double.infinity,
                color: Colors.red,
                child: Container(
                  alignment: Alignment.center,
                  width: double.infinity,
                  height: double.infinity,
                  color: Colors.blue,
                ))));
  }
}
{% endhighlight %}

## Padding組件

![img]({{site.imgurl}}/flutter/padding5.png)<br>

{% highlight dart linenos %}
class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
                width: double.infinity,
                height: double.infinity,
                color: Colors.red,
                child: Padding(
                  padding: EdgeInsets.all(20),
                  child: Container(
                    alignment: Alignment.center,
                    width: double.infinity,
                    height: double.infinity,
                    color: Colors.yellow,
                  ),
                ))));
  }
}
{% endhighlight %}
