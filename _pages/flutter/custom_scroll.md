---
title: CustomScrollView
date: 2026-04-06
keywords: flutter, CustomScrollView
---
## CustomScrollView
包裏許多滾動元件，但包裏的子元件都要以「Sliver」開頭。<br>
- SliverToBoxAdapter 可包裏Container、SizedBox。
- SliverPersistentHeader 吸頂元件
- SliverList 清單
- SliverGrid 網格元件
- SliverAppbar
- SliverPadding

## SliverToBoxAdapter
![img]({{site.imgurl}}/flutter/sliver_box.png)<br>

{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: CustomScrollView(
        slivers: [
          SliverToBoxAdapter(
            child: Container(
              height: 100,
              color: Colors.red,
            ),
          ),
        ],
      ),
    ));
  }
}
{% endhighlight %}

## SliverPersistentHeader 吸頂元件
delegate 屬性需要一個class繼承SliverPersistentHeaderDelegate
{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  ScrollController _controller = ScrollController();
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: CustomScrollView(
        slivers: [
          SliverToBoxAdapter(
            child: Container(
              height: 100,
              color: Colors.red,
            ),
          ),
          // 吸頂元件
          SliverPersistentHeader(
            // delegate
          	delegate: MyCategory(),
            // 吸頂功能
            pinned: true,
          )
        ],
      ),
    ));
  }
}
{% endhighlight %}
### delegate
這個功能是當滑到下面，上面的Head會收合高度變40，滑回最上面，又會展開高度變80。<br>

以下是主要四個要實作的方法，分別是build()、maxExtent、minExtent、shouldRebuild()。<br>
- build() 可以收合的子元素
- maxExtent 最大展開高度
- minExtent 最小收合高度
- shouldRebuild 是否每一次重新Load都要建構子元素。

展開:<br>
![img]({{site.imgurl}}/flutter/sliver1.png)<br>

收合:<br>
![img]({{site.imgurl}}/flutter/sliver2.png)<br>

{% highlight dart linenos %}
class MyCategory extends SliverPersistentHeaderDelegate {
  @override
  Widget build(BuildContext context, double shrinkOffset, bool overlapsContent) {
    // 可以收合的子元素
    return 子元素
  }

  // 最大展開高度
  @override
  double get maxExtent => 80;

  // 最小收合高度
  @override
  double get minExtent => 40;
  
  @override
  bool shouldRebuild(covariant SliverPersistentHeaderDelegate oldDelegate) {
    // TODO: implement shouldRebuild
    // 不用重新建構
    return false;
  }
{% endhighlight %}

## SliverList 清單列表
語法:
```
SliverList.separated(
  // 數量
  itemCount: 50,
  // 子元件 樣式
  itemBuilder: (BuildContext context, int index) {
    return 每一列的子元件
  },
  // 分隔線 樣式
  separatorBuilder: (BuildContext context, int index) {
    return 分隔線元件
  }
)
```

{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: CustomScrollView(
        slivers: [
          SliverToBoxAdapter(
            child: Container(
              height: 100,
              color: Colors.red,
            ),
          ),
          SliverPersistentHeader(
            delegate: MyCategory(),
            pinned: true,
          ),
          SliverList.separated(
              itemCount: 50,
              itemBuilder: (BuildContext context, int index) {
                return Container(
                  height: 100,
                  alignment: Alignment.center,
                  color: Colors.green,
                  child: Text('Category $index'),
                );
              },
              separatorBuilder: (BuildContext context, int index) {
                return Container(
                  height: 10,
                  color: Colors.white,
                );
              })
        ],
      ),
    ));
  }
}
{% endhighlight %}

## SliverToBoxAdapter SizedBox間隔
需要間隔，使用SizeBox，但要用SliverToBoxAdapter來包裝。<br>
{% highlight dart linenos %}
SliverToBoxAdapter(
	child: SizedBox(height: 20),
),
{% endhighlight %}

![img]({{site.imgurl}}/flutter/sliver3.png)<br>

{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: CustomScrollView(
        slivers: [
          // 標題
          SliverToBoxAdapter(
            child: Container(
              height: 100,
              color: Colors.red,
            ),
          ),
          // 間隔
          SliverToBoxAdapter(
            child: SizedBox(height: 20),
          ),
          // 吸頂元件
          SliverPersistentHeader(
            delegate: MyCategory(),
            pinned: true,
          ),
          // 間隔
          SliverToBoxAdapter(
            child: SizedBox(height: 20),
          ),
          // 清單元件
          SliverList.separated(
              itemCount: 50,
              itemBuilder: (BuildContext context, int index) {
                return Container(
                  height: 100,
                  alignment: Alignment.center,
                  color: Colors.green,
                  child: Text('Category $index'),
                );
              },
              separatorBuilder: (BuildContext context, int index) {
                return Container(
                  height: 10,
                  color: Colors.white,
                );
              })
        ],
      ),
    ));
  }
}
{% endhighlight %}

## SliverGrid 網格
![img]({{site.imgurl}}/flutter/sliver4.png)<br>
{% highlight dart linenos %}
class _MainPageState extends State<MainPage> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: CustomScrollView(
        slivers: [
          SliverToBoxAdapter(
            child: Container(
              height: 100,
              color: Colors.red,
            ),
          ),
          SliverToBoxAdapter(
            child: SizedBox(height: 20),
          ),
          SliverPersistentHeader(
            delegate: MyCategory(),
            pinned: true,
          ),
          SliverToBoxAdapter(
            child: SizedBox(height: 20),
          ),
          // 網格
          SliverGrid.count(
            crossAxisCount: 2,  // 欄數
            mainAxisSpacing: 10,  // 上下間隔
            crossAxisSpacing: 10, // 左右間隔
            children: List.generate(
              10,
              (index) => Container(
                height: 100,
                alignment: Alignment.center,
                color: Colors.green,
                child: Text('Category $index'),
              ),
            ),
          ),
        ],
      ),
    ));
  }
}
{% endhighlight %}