---
title: MaterialPageRoute
date: 2026-04-14
keywords: flutter, navigator, MaterialPageRoute
---
Navigator 需要包在 MaterialApp() 裡面，才可以使用。

導到某一個頁面語法:
```
Navigator.push(context,
                  MaterialPageRoute(builder: (context) => DetailPage()));
```

回到上一頁:
```
Navigator.pop(context);
```

![img]({{site.imgurl}}/flutter/nav1.png)<br>

![img]({{site.imgurl}}/flutter/nav2.png)<br>

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
  	// Navigator 需要包在 MaterialApp() 裡面，才可以使用。
    return MaterialApp(
      home: ListPage(),
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
              Navigator.push(context,
                  MaterialPageRoute(builder: (context) => DetailPage()));
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
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Detail Page'),
      ),
      body: Center(
        child: TextButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Back'),
        ),
      ),
    );
  }
}

{% endhighlight %}

## 傳遞參數
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
      home: ListPage(),
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
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) => DetailPage(id: index)));
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
  final int? id;
  const DetailPage({Key? key, this.id}) : super(key: key);

  @override
  _DetailPageState createState() => _DetailPageState();
}

class _DetailPageState extends State<DetailPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Detail Page'),
      ),
      body: Center(
        child: TextButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: Text('Back to List Page, id: ${widget.id}'),
        ),
      ),
    );
  }
}

{% endhighlight %}

