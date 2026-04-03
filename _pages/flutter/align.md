---
title: Center, Align
date: 2026-04-01
keywords: flutter, widget, Center, Align
---
## Center
Center 沒有寬(width)高(height)屬性，若要有寬高屬性，child要包Container。

![img]({{site.imgurl}}/flutter/center.png)<br>

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
      body: Center(
        child: Container(
            width: 100,
            height: 100,
            color: Colors.red,
            child: Center(child: Text("Hello World"))),
      ),
    ));
  }
}

{% endhighlight %}

## Align
### Align 屬性

![img]({{site.imgurl}}/flutter/align_attr.png)<br>

{% highlight dart linenos %}
class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
                alignment: Alignment.center,
                width: 100,
                height: 100,
                color: Colors.red,
                child: Text('hello world'))));
  }
}
{% endhighlight %}
### Align 佈局元件

|屬性|說明|
|:----:|:---------------------|
|alignment|子組件在父組件的對齊方式|
|widthFactor|父組件的寬度 = Align 中的子組件 `*` widthFactor|
|heightFactor|父組件的高度 = Align 中的子組件 `*` heightFactor|

![img]({{site.imgurl}}/flutter/align.png)<br>

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
            body: Container( // 父組件
                color: Colors.red,
                // 子組件
                child: Align( 
                    alignment: Alignment.center,
                    widthFactor: 3,
                    heightFactor: 3,
                    // Align 中的子組件
                    child: Container(
                      color: Colors.blue,
                      width: 100,
                      height: 100,
                    )))));
  }
}

{% endhighlight %}