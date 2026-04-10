---
title: Send Msg
date: 2026-04-10
keywords: flutter, widget
---
要傳的訊息變數前面要加final，類型可以是可選參數?，也可以使用required。<br>
可選參數是參數可填可不填，但required，參數一定要填。<br>
可選參數?語法
```
  final String? message;
  const Child({Key? key, this.message}) : super(key: key);
```

required 必要參數，必要參數就類型後面不用使用問號?。
```
  final String message;
  const Child({Key? key, required this.message}) : super(key: key);
```

## 無狀態子組件
{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Center(
        child: Child(message: 'Hello World'),
      ),
    ),
  ));
}

class Child extends StatelessWidget {
  final String? message;
  const Child({Key? key, this.message}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text("$message"),
    );
  }
}
{% endhighlight %}

## 有狀態子組件
- 繼承 StatefulWidget 對外類別處理傳遞過來的參數。
- 繼承 State 內部類別使用「widget.訊息變數」取得變數。

<br>
語法
```
${widget.message}
```

{% highlight dart linenos %}
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    home: Scaffold(
      body: Center(
        child: Child(message: 'Hello World'),
      ),
    ),
  ));
}

class Child extends StatefulWidget {
  final String? message;
  const Child({Key? key, this.message}) : super(key: key);

  @override
  State<Child> createState() => _ChildState();
}

class _ChildState extends State<Child> {
  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text("${widget.message}"),
    );
  }
}
{% endhighlight %}

## 有狀態傳送Lambda給子組件
- MainPage 為有狀態
- Child 為有狀態

在子組件宣告Lambda，Lambda類型是Function，要接收參數。<br>
```
final Function(參數) 函式名稱;
final Function(int index) delFood;
```

將Lambda加入到建構子參數。
{% highlight dart linenos %}
  const Child(
      {Key? key,
      required this.food,
      required this.index,
      required this.delFood})  // 函式名稱作為參數
      : super(key: key);
{% endhighlight %}

呼叫Lambda
```
widget.函式名稱(參數);
widget.delFood(widget.index);
```

定義Lambda
```
(參數) {
	要做的事
}

(int index) {
	list.removeAt(index);
    setState(() {});  // setState 更新變數更新介面
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
  List<String> list = ["牛肉麵", "魯肉飯", "排骨飯", "水餃"];
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: GridView.count(
              crossAxisCount: 2,
              mainAxisSpacing: 10,
              crossAxisSpacing: 10,
              children: List.generate(
                  list.length,
                  (index) => Child(
                        food: list[index],
                        index: index,
                        // 參數為Lambda
                        delFood: (int index) {
                          // 刪除
                          list.removeAt(index);
                          // 更新變數、更新介面
                          setState(() {});
                        },
                      ))),
        ),
      ),
    );
  }
}

class Child extends StatefulWidget {
  final String food;
  final int index;
  // 宣告Lambda
  final Function(int index) delFood;
  const Child(
      {Key? key,
      required this.food,
      required this.index,
      required this.delFood})  // Lambda作為建構子參數
      : super(key: key);

  @override
  State<Child> createState() => _ChildState();
}

class _ChildState extends State<Child> {
  @override
  Widget build(BuildContext context) {
    return Stack(
        // 超重要，把icon放在最右上角
        alignment: Alignment.topRight,
        children: [
          Container(
            color: Colors.blue,
            alignment: Alignment.center,
            child: Text("${widget.food}"),
          ),
          IconButton(
            icon: Icon(Icons.delete),
            onPressed: () {
              // 呼叫Lambda，並傳入參數
              widget.delFood(widget.index);
            },
          )
        ]);
  }
}

{% endhighlight %}