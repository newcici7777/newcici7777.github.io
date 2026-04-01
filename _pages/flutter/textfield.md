---
title: TextField
date: 2026-04-01
keywords: flutter, widget, TextField
---
簡單TextField程式碼:
{% highlight dart linenos %}
class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: Container(
          height: double.infinity,
          width: double.infinity,
          color: Colors.yellow,
          child: Column(
            children: [
              TextField(),
              TextField(),
              TextField(),
              TextButton(onPressed: () {}, child: Text("送出"))
            ],
          )),
    ));
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
|:----:|----:|:---------------------|
|contentPadding|EdgeInsets|內部間距|
|hintText|String|提示文字|
|fillColor|Colors|顏色|
|border|OutlineInputBorder|輸入框邊框|

### OutlineInputBorder 屬性說明

|屬性|類別|說明|
|:----:|----:|:---------------------|
|borderSide|BorderSide|none為無邊框|
|borderRadius|BorderRadius|輸入框圓角|

![img]({{site.imgurl}}/flutter/textfield1.png)<br>

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
          padding: EdgeInsets.all(20.0),
          height: double.infinity,
          width: double.infinity,
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
          )),
    ));
  }
}

{% endhighlight %}