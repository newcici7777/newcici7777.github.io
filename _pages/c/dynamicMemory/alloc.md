---
title: calloc, realloc
date: 2025-09-10
keywords: c++, calloc, realloc 
---
## calloc
建立陣列記憶體空間。
### 語法
{% highlight c++ linenos %}
void* calloc(unsigned int count,unsigned int size)
{% endhighlight %}

- 回傳值 void\* 記憶體起始位址
- count數量 陣列大小
- size 每一格元素大小

下圖中，count數量是20，size是4byte。<br>

![img]({{site.imgurl}}/c++/calloc.png)<br>

{% highlight c++ linenos %}
int main() {
  int count = 20;
  // p 指向起始位址
  int* p = (int * )calloc(count, 4);
  // 記錄p指標一開始的起始位址
  int* p2 = p;
  for (int i = 0; i < count; i++) {
    // 記憶體位址的值，指派為i變數
    * p = i;
    // p = p + 1，每移動一次，p的位址也會更改
    p++;
  }
  for (int i = 0; i < count; i++) {
    // 使用p2印出值
    cout << * (p2 + i) << ", ";
  }
  cout << endl;
  // 記憶體位址回收
  // free要用起始位址 p2記錄起始位址
  free(p2);
  // p2指標設為null
  p2 = nullptr;
  return 0;
}
{% endhighlight %}
```
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 
```

## realloc
原有malloc或calloc分配的記憶體空間，再次重新分配記憶體大小。
### 語法
{% highlight c++ linenos %}
void* calloc(void* p,unsigned int size)
{% endhighlight %}

- 回傳值 void\* 記憶體起始位址
- p 原有malloc或calloc分配的指標
- size 記憶體大小

{% highlight c++ linenos %}
int main() {
  int count = 20;
  // p 指向起始位址
  // 原本大小10 * 4 = 40
  int* p = (int * )calloc(10, 4);
  // 重新分配大小
  p = (int* ) realloc(p, count * sizeof(int));
  // 記錄p指標一開始的起始位址
  int* p2 = p;
  for (int i = 0; i < count; i++) {
    // 記憶體位址的值，指派為i變數
    * p = i;
    // p = p + 1，每移動一次，p的位址也會更改
    p++;
  }
  for (int i = 0; i < count; i++) {
    // 使用p2印出值
    cout << * (p2 + i) << ", ";
  }
  cout << endl;
  // 記憶體位址回收
  // free要用起始位址 p2記錄起始位址
  free(p2);
  // p2指標設為null
  p2 = nullptr;
  return 0;
}
{% endhighlight %}
```
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 
```