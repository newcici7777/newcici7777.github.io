---
title: Flex Expanded
date: 2026-04-01
keywords: flutter, widget, Flex
---
Flex 組件通常與 Expanded 搭配。<br>

Flex:<br>

|屬性|說明|
|:----:|:---------------------|
|direction方向|Axis.vertical垂直，Axis.horizontal水平|
|mainAxisAlignment主軸|跟 direction 一樣|
|crossAxisAlignment交叉軸|direction的相反|
|children|子元素|

Expanded flex屬性，等比例分配

以下direction方向是垂直，二個Expanded組件的flex為1，代表均分。
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

![img]({{site.imgurl}}/compose/flex1.png)<br>

以下direction為水平，二個Expanded組件占的空間為1:2，也就是紅色占1/3，綠色占2/3。<br>
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

![img]({{site.imgurl}}/compose/flex2.png)<br>