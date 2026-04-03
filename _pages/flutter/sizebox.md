---
title: SizedBox
date: 2026-04-03
keywords: flutter, widget,Sizebox
---
SizeBox是產生間隔。<br>

## Row SizedBox(width)
Row 之間的間隔，SizedBox要設定 width。<br>

![img]({{site.imgurl}}/flutter/sizedbox1.png)<br>

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
      color: Colors.yellow,
      child: Row(
        children: [
          Container(
            width: 100,
            height: 100,
            color: Colors.blue,
          ),
          SizedBox(
            width: 20,
          ),
          Container(
            width: 100,
            height: 100,
            color: Colors.green,
          ),
          SizedBox(
            width: 20,
          ),
          Container(
            width: 100,
            height: 100,
            color: Colors.red,
          ),
        ],
      ),
    )));
  }
}

{% endhighlight %}

## Column SizedBox(height)
Column 之間的間隔，SizedBox要設定 height<br>

![img]({{site.imgurl}}/flutter/sizedbox2.png)<br>

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
      color: Colors.yellow,
      child: Column(
        children: [
          Container(
            width: 100,
            height: 100,
            color: Colors.blue,
          ),
          SizedBox(
            height: 20,
          ),
          Container(
            width: 100,
            height: 100,
            color: Colors.green,
          ),
          SizedBox(
            height: 20,
          ),
          Container(
            width: 100,
            height: 100,
            color: Colors.red,
          ),
        ],
      ),
    )));
  }
}

{% endhighlight %}