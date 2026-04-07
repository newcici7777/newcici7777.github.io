---
title: SingleChildScrollView
date: 2026-04-04
keywords: flutter, SingleChildScrollView
---
SingleChildScrollView 包住一個子元件(只有一個child)，讓子元件可以滾動畫面。<br>

- 一次性建立所有子元件，不會隨著下滑而逐漸建立子元件。
- 因為只能一個子元件，所以會使用可以包裏住多個列或欄的子元件的佈局組件，如: Column, Row。
- 預設滾動方向(scrollDirection)是垂直(Axis.vertical)，也可以設成水平(Axis.horizontal)

## 包住一個子元件
![img]({{site.imgurl}}/flutter/single_scroll1.png)<br>

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatefulWidget {
  MainPage({Key? key}) : super(key: key);

  @override
  _MainPageState createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: SingleChildScrollView(
      child: Column(
        children: List.generate(10, (index) {
          return Container(
            margin: EdgeInsets.only(top: 10.0),
            width: double.infinity,
            color: Colors.blue,
            height: 100,
            alignment: Alignment.center,
            child: Text("item $index"),
          );
        }),
      ),
    )));
  }
}
{% endhighlight %}
