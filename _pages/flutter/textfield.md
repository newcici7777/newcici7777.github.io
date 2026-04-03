---
title: TextField
date: 2026-04-01
keywords: flutter, widget, TextField
---
## TextField要建立StatefulWidget
直接在畫面輸入stateful，就會有選單跳出來。<br>
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
    return Container(
       child: null,
    );
  }
}
{% endhighlight %}

## 簡單TextField程式碼
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
      width: double.infinity,
      height: double.infinity,
      color: Colors.yellow,
      child: Column(
        children: [
          TextField(),
          TextField(),
          TextField(),
          TextButton(onPressed: () {}, child: Text("送出"))
        ],
      ),
    )));
  }
}

{% endhighlight %}

## InputDecoration 輸入框樣式
語法
```
TextField(decoration: InputDecoration())
```

### InputDecoration 屬性說明

|屬性|類別|說明|
|:----:|:----:|:---------------------|
|contentPadding|EdgeInsets|內部間距|
|hintText|String|提示文字|
|fillColor|Colors|顏色|
|border|OutlineInputBorder|輸入框邊框|

### OutlineInputBorder 屬性說明

|屬性|類別|說明|
|:----:|:----:|:---------------------|
|borderSide|BorderSide|none為無邊框|
|borderRadius|BorderRadius|輸入框圓角|

![img]({{site.imgurl}}/flutter/textfield1.png)<br>

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
      padding: EdgeInsets.all(20.0),
      width: double.infinity,
      height: double.infinity,
      color: Colors.yellow,
      child: Column(
        children: [
          TextField(
              decoration: InputDecoration(
                  contentPadding: EdgeInsets.only(left: 20.0, right: 10),
                  hintText: "請輸入姓名",
                  fillColor: Colors.grey,
                  filled: true,
                  border: OutlineInputBorder(
                      borderSide: BorderSide.none,
                      borderRadius: BorderRadius.circular(10.0)))),
          TextButton(onPressed: () {}, child: Text("送出"))
        ],
      ),
    )));
  }
}

{% endhighlight %}

## controller
TextField有一個屬性為 controller，類別是TextEditingController。<br>

語法:
```
TextEditingController _controller = TextEditingController();
```

搭配TextButtom可以收到使用者輸入的內容`_controller.text`。
{% highlight dart linenos %}
TextButton(onPressed: () {
             print(_controller.text);
           }, child: Text("送出"))
{% endhighlight %}


![img]({{site.imgurl}}/flutter/textfield2.png)<br>

![img]({{site.imgurl}}/flutter/textfield3.png)<br>

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
  TextEditingController _controller = TextEditingController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
      padding: EdgeInsets.all(20.0),
      width: double.infinity,
      height: double.infinity,
      color: Colors.yellow,
      child: Column(
        children: [
          TextField(
              controller: _controller,
              decoration: InputDecoration(
                  contentPadding: EdgeInsets.only(left: 20.0, right: 10),
                  hintText: "請輸入姓名",
                  fillColor: Colors.grey,
                  filled: true,
                  border: OutlineInputBorder(
                      borderSide: BorderSide.none,
                      borderRadius: BorderRadius.circular(10.0)))),
          TextButton(
              onPressed: () {
                print(_controller.text);
              },
              child: Text("送出"))
        ],
      ),
    )));
  }
}
{% endhighlight %}

## onChanged onSubmitted
TextField有onChanged 與 onSubmitted 屬性。<br>

### onChanged
onChanged()是輸入TextField有變化就會收到。<br>
```
onChanged: (value) {
  print("change: $value");
}
```

![img]({{site.imgurl}}/flutter/textfield4.png)<br>

### onSubmitted
onSubmitted，在Web瀏覽器，當使用者在TextField按下鍵盤enter，就會收到。<br>
```
onSubmitted: (value) {
  print("submit: $value");
}
```

![img]({{site.imgurl}}/flutter/textfield5.png)<br>

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
  TextEditingController _controller = TextEditingController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: Container(
      padding: EdgeInsets.all(20.0),
      width: double.infinity,
      height: double.infinity,
      color: Colors.yellow,
      child: Column(
        children: [
          TextField(
              controller: _controller,
              onChanged: (value) {
                print("change: $value");
              },
              onSubmitted: (value) {
                print("submit: $value");
              },
              decoration: InputDecoration(
                  contentPadding: EdgeInsets.only(left: 20.0, right: 10),
                  hintText: "請輸入姓名",
                  fillColor: Colors.grey,
                  filled: true,
                  border: OutlineInputBorder(
                      borderSide: BorderSide.none,
                      borderRadius: BorderRadius.circular(10.0)))),
          TextButton(
              onPressed: () {
                print(_controller.text);
              },
              child: Text("送出"))
        ],
      ),
    )));
  }
}

{% endhighlight %}