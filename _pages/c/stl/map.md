---
title: map
date: 2024-12-10
keywords: c++, pair, map
---

Prerequisites:

- [pair][1]
- [initializer list][2]
- [iterator][3]

## map介紹

include

```
#include <map>
```

map的值是pair

map是以紅黑樹(AVL tree)排序，所以把map的值印出來會是以排序好的方式印出。

## 初始化

語法
```
map(initializer_list<pair<K,V>> il);
```

{% highlight c++ linenos %}
  // 建立空的map
  map<int, string> m1;
  // 使用initializer初始化值
  // map(initializer_list<pair<K,V>>)
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  // 省略等於號寫法
  // map<int,string> m2 { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  // 使用括號
  // map<int,string> m2({ {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} });
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
{% endhighlight %}
```
6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 
```
## 使用[]印出元素
{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  cout << "m2[10] = " << m2[10] << endl;
{% endhighlight %}
```
m2[10] = Mary
```

若key不存在map中，則會新增空值
{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  cout << "m2[10] = " << m2[10] << endl;
  cout << "m2[22] = " << m2[22] << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
{% endhighlight %}
```
m2[10] = Mary
m2[22] = 
6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 22, 
```

## 新增修改
{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  cout << "修改前" << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
  // 新增
  m2[5] = "Eva";
  m2[4] = "Iric";
  // 修改
  m2[10] = "Yoyo";
  m2[9] = "Momo";
  cout << "修改後" << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
{% endhighlight %}
```
修改前
6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 
修改後
4,Iric 5,Eva 6,Ken 7,Alice 8,Juli 9,Momo 10,Yoyo 
```

## 插入insert

### 傳回值為void

語法
```
void insert(initializer_list<pair<K,V>> il);
```

{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  cout << "修改前" << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
  m2.insert({ {1, "Ben"}, {2, "Cindy"}, {3, "Rita"} });
  cout << "插入後" << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
{% endhighlight %}
```
修改前
6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 
插入後
1,Ben 2,Cindy 3,Rita 6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 
```

若插入的key已存在map中，則不會插入，也不會提示錯誤
{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  cout << "修改前" << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
  m2.insert({ {10, "Ben"}, {9, "Cindy"}, {3, "Rita"} });
  cout << "插入後" << endl;
  for (auto& val:m2) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
{% endhighlight %}
```
修改前
6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 
插入後
3,Rita 6,Ken 7,Alice 8,Juli 9,Bill 10,Mary 
```

### 傳回值為pair
語法
```
pair<iterator,bool> insert(initializer_list<pair<K,V>> il);
```
傳回值pair:first已插入的元素的iterator，second是插入的結果
{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  auto ret = m2.insert({5, "Julia"});
  if (ret.second == true) {
    cout << "成功" << ", first = " << ret.first->first
  << ", second = " << ret.first -> second << endl;
  } else {
    cout << "失敗" << endl;
  }
{% endhighlight %}
```
成功, first = 5, second = Julia
```

### 使用emplace
```
pair<iterator,bool> emplace(initializer_list<pair<K,V>> il);
```
傳回值跟先前的一樣，不同的是使用emplace可以原地建構。

## 使用iterator建立map
{% highlight c++ linenos %}
  map<int,string> m2 = { {10, "Mary"}, {9, "Bill"}, {7, "Alice"}, {8, "Juli"}, {6, "Ken"} };
  m2.insert({ {1, "Ben"}, {2, "Cindy"}, {3, "Rita"} });
  // 使用iterator建立map
  auto first = m2.begin();
  first++;
  auto last = m2.end();
  last--;
  map<int, string> m3(first, last);
  for (auto & val:m3) {
    cout << val.first << "," << val.second << " ";
  }
  cout << endl;
{% endhighlight %}
```
2,Cindy 3,Rita 6,Ken 7,Alice 8,Juli 9,Bill 
```



[1]: {% link _pages/c/stl/pair.md %}
[2]: {% link _pages/c/c11/init_list.md %}
[3]: {% link _pages/c/stl/iterator.md %}
