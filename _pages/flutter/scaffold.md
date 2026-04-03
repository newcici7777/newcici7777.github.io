---
title: Scaffold() 骨架
date: 2026-03-31
keywords: flutter, widget, Scaffold()
---

|屬性|說明|
|:----:|:---------------------|
|appBar|app標題|
|body|主要內容|
|bottomNavigationBar|底部導覽|
|backgroundColor|背景顏色|
|floatingActionButton|浮動按鈕|

![img]({{site.imgurl}}/flutter/scaffold.png)<br>

{% highlight dart linenos %}
void main() {
  runApp(MaterialApp(
    title: 'Flutter Base',
    theme: ThemeData(scaffoldBackgroundColor: Colors.blue),
    home: Scaffold(
      appBar: AppBar(
        title: Text('AppBar'),
      ),
      body: Container(
        child: Center(
          child: Text('Body'),
        ),
      ),
      bottomNavigationBar: Container(
        height: 50,
        child: Center(
          child: Text('Bottom Navigation Bar'),
        ),
      ),
    ),
  ));
}
{% endhighlight %}

