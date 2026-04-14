---
title: Named Routes
date: 2026-04-14
keywords: flutter, navigator,Named Routes
---
語法
{% highlight dart linenos %}
MaterialApp(
    initialRoute: "/list", // 首頁 
    routes: {
      "/list": (context) => ListPage(),  // 清單頁
      "/detail": (context) => DetailPage(),  // 詳細頁
    });
{% endhighlight %}

導至其它頁面
```
Navigator.pushNamed(context, "/detail");
```

完整程式碼
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
      "/detail": (context) => DetailPage(),
    });
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
              Navigator.pushNamed(context, "/detail");
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
            Navigator.pushNamed(context, "/list");
          },
          child: Text('Back'),
        ),
      ),
    );
  }
}
{% endhighlight %}

### 傳遞參數
```
Navigator.pushNamed(context, route, arguments: 參數);
Navigator.pushNamed(context, "/detail", arguments: {"id": index});
```

### 接收參數
接收參數要在initState()方法接收，並且要使用`Future.microtask()`來接收參數。<br>
```
@override
void initState() {
  Future.microtask(() {
	// 接收參數
  });	
}
```

接收參數語法
```
變數 = ModalRoute.of(context)!.settings.arguments;
```

{% highlight dart linenos %}
  @override
  void initState() {
    super.initState();
    Future.microtask(() {
      Map<String, dynamic> args =
          ModalRoute.of(context)!.settings.arguments as Map<String, dynamic>;
      print(args["id"]);
    });
  }
{% endhighlight %}

### 完整程式碼
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
    return MaterialApp(initialRoute: "/list", routes: {
      "/list": (context) => ListPage(),
      "/detail": (context) => DetailPage(),
    });
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
              Navigator.pushNamed(context, "/detail", arguments: {"id": index});
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

{% endhighlight %}