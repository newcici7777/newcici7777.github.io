---
title: Flex Expanded
date: 2026-04-01
keywords: flutter, widget, Flex, Expanded
---
Flex 組件通常與 Expanded 搭配。<br>

Flex:<br>

|屬性|說明|
|:----:|:---------------------|
|direction 拉伸方向|Axis.vertical垂直，Axis.horizontal水平|
|mainAxisAlignment主軸|跟 direction 一樣|
|crossAxisAlignment交叉軸|direction的相反|
|children|子元素|

Expanded flex屬性，等比例分配空間。

以下direction方向是垂直拉伸，二個Expanded組件的flex為1，占據空間比例為1:1，代表垂直空間均分。
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
        width: double.infinity,
        height: double.infinity,
        child: Flex(
          direction: Axis.vertical,
          children: [
            Expanded(
                flex: 1,
                child: Container(color: Colors.red, width: 100, height: 100)),
            Expanded(
                flex: 1,
                child: Container(color: Colors.green, width: 100, height: 100))
          ],
        ),
      ),
    ));
  }
}
{% endhighlight %}

![img]({{site.imgurl}}/flutter/flex1.png)<br>

以下direction為水平拉伸，二個Expanded組件占的空間為1:2，也就是紅色占1/3，綠色占2/3。<br>
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
        width: double.infinity,
        height: double.infinity,
        child: Flex(
          direction: Axis.horizontal,
          children: [
            Expanded(
                flex: 1,
                child: Container(color: Colors.red, width: 100, height: 100)),
            Expanded(
                flex: 2,
                child: Container(color: Colors.green, width: 100, height: 100))
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}

![img]({{site.imgurl}}/flutter/flex2.png)<br>

以下的程式碼，因為只有一個Expanded，所以不用設定flex，扣掉上面藍色與下方紅色，剩下的就是垂直拉伸空間。<br>
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
        child: Flex(
          direction: Axis.vertical,
          children: [
            Container(
              color: Colors.blue,
              height: 100,
            ),
            Expanded(child: Container(color: Colors.grey)),
            Container(
              color: Colors.red,
              height: 100,
            ),
          ],
        ),
      ),
    ));
  }
}

{% endhighlight %}
![img]({{site.imgurl}}/flutter/expend1.png)<br>