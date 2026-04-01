---
title: Stack, Positioned
date: 2026-04-01
keywords: flutter, widget, Stack, Positioned
---
## Stack 堆疊
Stack，中文是堆疊。<br>

|屬性|說明|
|:----:|:---------------------|
|alignment|對齊，預設為左上角|
|children|子元素|

以下程式碼執行完後，會一層一層堆疊，最後面的Container放在最上面。<br>
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
        child: Stack(
          children: [
            Container(
              color: Colors.grey,
              width: 300,
              height: 300,
            ),
            Container(
              color: Colors.red,
              width: 200,
              height: 200,
            ),
            Container(
              color: Colors.blue,
              width: 100,
              height: 100,
            ),
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}

![img]({{site.imgurl}}/flutter/stack1.png)<br>

### 置中
增加置中對齊
```
alignment: Alignment.center,
```
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
        child: Stack(
          alignment: Alignment.center,
          children: [
            Container(
              color: Colors.grey,
              width: 300,
              height: 300,
            ),
            Container(
              color: Colors.red,
              width: 200,
              height: 200,
            ),
            Container(
              color: Colors.blue,
              width: 100,
              height: 100,
            ),
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}

![img]({{site.imgurl}}/flutter/stack2.png)<br>

## Positioned 位置
Stack要包住Positioned。<br>
Positioned，中文是位置，子元素要放在畫面中那個位置。<br>
下面程式碼，執行後上、下、左、右有四個小方塊。<br>
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
        color: Colors.amber,
        width: double.infinity,
        height: double.infinity,
        child: Stack(
          children: [
            Positioned(
              top: 0,
              left: 0,
              child: Container(
                width: 50,
                height: 50,
                color: Colors.red,
              ),
            ),
            Positioned(
              top: 0,
              right: 0,
              child: Container(
                width: 50,
                height: 50,
                color: Colors.blue,
              ),
            ),
            Positioned(
              bottom: 0,
              left: 0,
              child: Container(
                width: 10,
                height: 10,
                color: Colors.red,
              ),
            ),
            Positioned(
              bottom: 0,
              right: 0,
              child: Container(
                width: 50,
                height: 50,
                color: Colors.red,
              ),
            ),
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}

![img]({{site.imgurl}}/flutter/stack_pos.png)<br>

## Positioned 拉伸(自適應)
### right 0 left 0 左右拉伸
![img]({{site.imgurl}}/flutter/stack_pos2.png)<br>
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
        child: Stack(
          alignment: Alignment.center,
          children: [
            Positioned(
              right: 0,
              left: 0,
              child: Container(
                color: Colors.grey,
                width: 300,
                height: 300,
              ),
            ),
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}

### top 0 bottom 0 上下拉伸
![img]({{site.imgurl}}/flutter/stack_pos3.png)<br>
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
        child: Stack(
          alignment: Alignment.center,
          children: [
            Positioned(
              top: 0,
              bottom: 0,
              child: Container(
                color: Colors.grey,
                width: 300,
                height: 300,
              ),
            ),
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}