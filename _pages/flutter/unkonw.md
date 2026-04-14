---
title: onGenerateRoute onUnknownRoute
date: 2026-04-14
keywords: flutter, navigator,onGenerateRoute, onUnknownRoute
---
- onGenerateRoute 沒有在routes清單，都進來這邊。
- onUnknownRoute 沒在routes，也沒在onGenerateRoute，都進來這邊。

![img]({{site.imgurl}}/flutter/unkonw.png)<br>

{% highlight dart linenos %}
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(MainPage());
}

class MainPage extends StatelessWidget {
  const MainPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: "/list",
      routes: {
        "/list": (context) => ListPage(),
        // 假裝把detail給註解
        //"/detail": (context) => DetailPage(),
      },
      onGenerateRoute: (settings) {
        if (settings.name == "/detail") {
          // 使用MaterialPageRoute 需要使用MaterialPageRoute傳遞參數的方式
          return MaterialPageRoute(builder: (context) => DetailPage());
        }
      },
      onUnknownRoute: (settings) {
      	// 若路徑不在routes，也沒在onGenerateRoute被處理，就會來到這裡
        return MaterialPageRoute(builder: (context) => NotFound());
      },
    );
  }
}

class ListPage extends StatefulWidget {
  ListPage({Key? key}) : super(key: key);

  @override
  _ListPageState createState() => _ListPageState();
}

class _ListPageState extends State<ListPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('List Page'),
      ),
      body: ListView.builder(
        itemCount: 10,
        itemBuilder: (context, index) {
          return GestureDetector(
            onTap: () {
              Navigator.pushNamed(context, "/abc", arguments: {"id": index});
            },
            child: Container(
              color: Colors.blue,
              margin: EdgeInsets.only(top: 10),
              height: 80,
              alignment: Alignment.center,
              child: Text('item $index',
                  style: TextStyle(color: Colors.white, fontSize: 20)),
            ),
          );
        },
      ),
    );
  }
}

class DetailPage extends StatefulWidget {
  DetailPage({Key? key}) : super(key: key);

  @override
  _DetailPageState createState() => _DetailPageState();
}

class _DetailPageState extends State<DetailPage> {
  int? _id;
  @override
  void initState() {
    super.initState();
    Future.microtask(() {
      Map<String, dynamic> args =
          ModalRoute.of(context)!.settings.arguments as Map<String, dynamic>;
      print("args:${args["id"]}");
      // 參數設定在變數中
      _id = args["id"] as int?;
      // 更新變數
      setState(() {});
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Detail Page'),
      ),
      body: Center(
        child: TextButton(
          onPressed: () {
            Navigator.pushNamed(context, "/list");
          },
          child: Text('item index = $_id, Back to List Page'),
        ),
      ),
    );
  }
}

class NotFound extends StatelessWidget {
  const NotFound({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      child: Text('404 Not Found'),
    );
  }
}

{% endhighlight %}