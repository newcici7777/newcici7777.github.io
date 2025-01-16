---
title: 氣泡排序模板
date: 2024-12-01
keywords: c++, template, sort
---

Prerequisites:

- [氣泡排序][1]

## 由小到大排序

傳回true的條件，左邊的值小於右邊的值

{% highlight c++ linenos %}
template<typename T>
bool CompareASC(const T& left, const T& right) {
  return left < right;
}
{% endhighlight %}

## 由大到小排序

傳回true的條件，左邊的值大於右邊的值
{% highlight c++ linenos %}
template<typename T>
bool CompareDESC(const T& left, const T& right) {
  return left > right;
}
{% endhighlight %}

## 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T>
bool CompareASC(const T& left, const T& right) {
  return left < right;
}
template<typename T>
bool CompareDESC(const T& left, const T& right) {
  return left > right;
}
template<typename T, typename Compare>
void BubbleSort(const T first, const T last, Compare comp) {
  while (true) {
    // 判斷容器所有容器原本就排好
    bool bswap = false;
    // it指標移到第一個元素位址
    for (auto it = first; ; ) {
      // 左邊的元素
      auto left = it;
      // 移到下個位址
      it++;
      // 右邊的元素
      auto right = it;
      // right是最後一個元素的下一個位址
      if (right == last) break;
      // 若二個元素已經是排好就回到for判斷條件
      if (comp(*left, *right) == true) continue;
      // 交換
      auto temp = *right;
      *right = *left;
      *left = temp;
      // 代表有交換
      bswap = true;
    }
    // 若沒有交換代表全部元素都已經是有序的。
    if (bswap == false) break;
  }
}
int main() {
  vector<string> v = {"05", "04", "03", "02", "01"};
  BubbleSort(v.begin(), v.end(), CompareASC<string>);
  for (auto val: v) {
    cout << val << " ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}
```
01 02 03 04 05 
```

## 改用物件函式

效果與函式指標是相同。

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T>
class CompareASC {
 public:
  bool operator()(const T& left, const T& right) {
    return left < right;
  }
};
template<typename T>
class CompareDESC {
 public:
  bool operator()(const T& left, const T& right) {
    return left > right;
  }
};
template<typename T, typename Compare>
void BubbleSort(const T first, const T last, Compare comp) {
  while (true) {
    // 判斷容器所有容器原本就排好
    bool bswap = false;
    // it指標移到第一個元素位址
    for (auto it = first; ; ) {
      // 左邊的元素
      auto left = it;
      // 移到下個位址
      it++;
      // 右邊的元素
      auto right = it;
      // right是最後一個元素的下一個位址
      if (right == last) break;
      // 若二個元素已經是排好就回到for判斷條件
      if (comp(*left, *right) == true) continue;
      // 交換
      auto temp = *right;
      *right = *left;
      *left = temp;
      // 代表有交換
      bswap = true;
    }
    // 若沒有交換代表全部元素都已經是有序的。
    if (bswap == false) break;
  }
}
int main() {
  vector<string> v = {"05", "04", "03", "02", "01"};
  BubbleSort(v.begin(), v.end(), CompareASC<string>());
  for (auto val: v) {
    cout << val << " ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}
```
05 04 03 02 01 
```

[1]: {% link _pages/c/dataStruct/bubbleSort.md %}
