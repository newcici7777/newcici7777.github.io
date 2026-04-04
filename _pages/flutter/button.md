---
title: GestureDetector 與 Button
date: 2026-04-03
keywords: flutter, widget, 
---
Button 有 ElevatedButton,TextButton,OutlineButton,FloatingActionButton, IconButton, Switch,Checkbox。<br>

都是用onPressed 屬性接收點擊事件。<br>

語法:
```
onPressed: 匿名函式
onPressed:() {}
```

## TextButton()
TextButton沒有自己的寬高，必須包在Container父元件，因為Container有寬高。<br>

|方法|說明|
|:----|:-------------|
|onPressed|點一下|

語法:<br>
```
onPressed:() {
}
```

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
            body: Container(
      color: Colors.yellow,
      child: Container(
          width: 50.0,
          height: 50.0,
          color: Colors.red,
          child: TextButton(
              onPressed: () {
                print("abcd");
              },
              child: Text("送出"))),
    )));
  }
}

{% endhighlight %}

## GestureDetector

|方法|說明|
|:----|:-------------|
|onTap|點一下|
|onDoubleTap|點二下|

語法:<br>
```
onTap: () {
  print("GestureDetector");
}
```

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
            body: Container(
      color: Colors.yellow,
      child: Container(
          width: 50.0,
          height: 50.0,
          color: Colors.red,
          child: GestureDetector(
              onTap: () {
                print("GestureDetector");
              },
              onDoubleTap: () {
                print("doubleTap");
              },
              child: Text("送出"))),
    )));
  }
{% endhighlight %}