---
title: 程式碼格式
date: 2024-11-15
keywords: c++, format
---

以下內容參考[google C++風格指南](https://www.cclo.idv.tw/google-cpp-styleguide-zhTW/formatting.html#class-format)

## 空白

每一句程式碼內縮2個空白

以下程式碼vector前面留有2個空白

{% highlight c++ linenos %}
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  return 0;
}
{% endhighlight %}

xcode把tab改為2個空白

![img]({{site.imgurl}}/xcode_c++/tab_space.png)

### for

for(){....}中的內縮仍是2個空白

{% highlight c++ linenos %}
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  for (auto it = v.crbegin(); it != v.crend(); it++) {
    //內縮2個空白
    cout << *it << ", ";
  }
  return 0;
}
{% endhighlight %}


for與圓括號()之間要有空白，圓括號()與左大括號{要有空白，並且左大括號{與for在同一行
{% highlight c++ linenos %}
  for () {	
  }
{% endhighlight %}


圓括號()與裡面的條件，開頭與結尾是沒空白
{% highlight c++ linenos %}
  for (int i = 0; i < 10; i++) {	
  }
{% endhighlight %}


分號;後留一個空白，=等於的前後留空白，<的前後留空白，++前後不用留空白
{% highlight c++ linenos %}
  for (int i = 0; i < 10; i++)
{% endhighlight %}

### 參數

大括號{...}裡面的值，逗號(,)右側留一個空白，左側不留空白，大括號{...}中，前後不留空白。

變數名v與類型留一個空白

=等於，前後留空白

{% highlight c++ linenos %}
  vector<int> v = {1, 2, 3, 4, 5};
{% endhighlight %}

還有很多沒寫完... 待續... 未完...