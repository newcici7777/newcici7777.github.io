---
title: iterator疊代器
date: 2024-11-14
keywords: c++, iterator
---

Prerequisites:

- [記憶體間隔計算][1]
- [函式模板][2]

## begin與end的位置

- iterator.begin指向容器的首元素記憶體位址
- iterator.end指向容器最後一個元素的`下一個`位址

圖中容器有10個元素，begin指向首元素，end指向最後一個元素的下一個位址
![img]({{site.imgurl}}/pointer/container1.jpg)

## find模板

### find在array寫法

{% highlight c++ linenos %}
/**
參數1 arr陣列位址
參數2 陣列大小
參數3 要尋找的值
 */
int* _find(int* arr, int n, const int& val) {
  for(int i = 0; i < n; i++) {
    //若找到與val相同的值，返回元素的記憶體位址
    if(arr[i] == val) return &arr[i];
  }
  //找不到就返回null
  return nullptr;
}
{% endhighlight %}

### 改寫成begin與end的參數

{% highlight c++ linenos %}
/**
參數1 開始位址
參數2 最後位址的下一個位址
參數3 要尋找的值
 */
int* _find(int* begin, int* end, const int& val) {
  for(int* iter = begin; iter != end; iter++) {
    //若找到與val相同的值，返回元素的記憶體位址
    if(*iter == val) return iter;
  }
  //找不到就返回null
  return nullptr;
}
{% endhighlight %}

### 支援鏈結串列的find

{% highlight c++ linenos %}
struct Node {
  int item;
  Node* next;
};
/**
參數1 開始位址
參數2 最後位址的下一個位址
參數3 要尋找的值
 */
Node* _find(Node* head, Node* end, const Node& val) {
  for(Node* iter = head; iter != end; iter = iter->next) {
    //若找到與val相同的值，返回元素的記憶體位址
    if(iter->item == val.item) return iter;
  }
  //找不到就返回null
  return nullptr;
}
{% endhighlight %}

### find模板程式碼

C++的容器(vector,list,string...)都有iterator，iterator包含begin與end，可以把不同容器的begin與end作為模板參數傳入模板函式。

注意!!模板中，找不到元素是返回end位址，而不是nullptr。

{% highlight c++ linenos %}
template<typename T1,typename T2>
T1 _find(T1 begin, T1 end, const T2 &val) {
  for(T1 iter = begin; iter != end; iter ++) {
    if(*iter == val) return iter;
  }
  //注意!!如果找不到就返回end位址
  return end;
}
{% endhighlight %}


## vector使用find模板

vector資料結構是陣列。

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
template<typename T1,typename T2>
T1 _find(T1 begin, T1 end, const T2 &val) {
  for(T1 iter = begin; iter != end; iter ++) {
    if(*iter == val) return iter;
  }
  return end;
}
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  vector<int>::iterator it1 = _find(v.begin(), v.end(), 4);
  if (it1 != v.end())
    cout << "找到" << *it1 << endl;
  else
    cout << "找不到" << endl;
  return 0;
}
{% endhighlight %}
```
找到4
```

## list使用find模板

list的資料結構是鏈結串列。

要記得include
```
#include <list>
```
{% highlight c++ linenos %}
int main() {
  list<int> list1 = {1, 2, 3, 4, 5};
  list<int>::iterator it1 = _find(list1.begin(), list1.end(), 4);
  if (it1 != list1.end())
    cout << "找到" << *it1 << endl;
  else
    cout << "找不到" << endl;
  return 0;
}
{% endhighlight %}
```
找到4
```

## 疊代器支持的運算子

疊代器模擬了C++中的指針，可以有++運算，用*（取值運算子，deference）或->來訪問容器中的元素。

- *iter 取值運算子
- iter->member 讀取當前物件的成員變數
- iter=iter1 指派給另一個疊代器
- iter==iter1 疊代器相等比較
- iter!=iter1 疊代器不等比較

## 疊代器分類

### 正向疊代器

只能++，不能--，只能往前遍歷，不能往後遍歷。例如資料結構為單向鏈結串列的list不能向後遍歷。

#### 宣告疊代器語法

- 可修改元素的疊代器
```
容器<元素類型>::iterator 疊代器名;
vector<int>::iterator it;
```
- 不可修改元素的疊代器
```
容器<元素類型>::const_iterator 疊代器名;
vector<int>::const_iterator it;
```

#### begin()

iterator移到容器第0個元素，回傳的iterator可以修改值
{% highlight c++ linenos %}
iterator begin();
{% endhighlight %}

回傳的iterator不可以修改值(有const)
{% highlight c++ linenos %}
const_iterator begin();
{% endhighlight %}

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
#include <list>
using namespace std;
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  //把iterator移到容器第0個元素
  vector<int>::iterator it = v.begin();
  //將第0個元素的值改成7
  *it = 7;
  //iterator移到下一個元素
  it++;
  //將第1個元素的值改為8
  *it = 8;

  for (vector<int>::const_iterator it = v.begin(); it != v.end(); it++) {
  cout << *it << ", ";
  }
  return 0;
}
{% endhighlight %}

```
7, 8, 3, 4, 5,
```

#### cbegin()

類型可以簡寫成auto，不可以修改值(有const)
```
const_iterator cbegin();
```

原本的程式碼
{% highlight c++ linenos %}
  for (vector<int>::const_iterator it = v.cbegin(); it != v.cend(); it++) {
  cout << *it << ", ";
  }
  cout << endl;
{% endhighlight %}

類型簡寫成auto
{% highlight c++ linenos %}
  for (auto it = v.cbegin(); it != v.cend(); it++) {
  cout << *it << ", ";
  }
  cout << endl;
{% endhighlight %}

#### end()

取得最後一個元素的`下一個`記憶體位址

可以修改值
{% highlight c++ linenos %}
iterator end();
{% endhighlight %}

不可以修改值
{% highlight c++ linenos %}
const_iterator end();
{% endhighlight %}

類型可簡寫成auto，不可以修改值
{% highlight c++ linenos %}
const_iterator cend();
{% endhighlight %}

### 雙向疊代器

疊代器可以往前遍歷，也可以向後遍歷，支援--(向後遍歷)的功能。擁有正向疊代器與反向疊代器的功能，正向疊代器上述已提過。

#### reverse_iterator反向疊代器

圖中容器有10個元素，rbegin指向「最後一個元素」的「起始位址」，rend指向「第 0 個元素的前一個位置」。

![img]({{site.imgurl}}/pointer/container2.jpg)

- 可修改元素的疊代器

{% highlight c++ linenos %}
容器<元素類型>::reverse_iterator 疊代器名;
vector<int>::reverse_iterator it;
{% endhighlight %}

- 不可修改元素的疊代器

{% highlight c++ linenos %}
容器<元素類型>::const_reverse_iterator 疊代器名;
vector<int>::const_reverse_iterator it;
{% endhighlight %}

#### rbegin()

返回的是一個反向迭代器，指向容器最後一個元素的位址。這樣可以從容器的最後一個元素開始反向遍歷。

可以修改iterator指向位址的值。

{% highlight c++ linenos %}
reverse_iterator rbegin();
{% endhighlight %}

#### crbegin()

類型可以簡化成auto，不可以修改iterator指向位址的值。

{% highlight c++ linenos %}
const_reverse_iterator crbegin();
{% endhighlight %}

#### rend()

返回的則是指向容器「第 0 個元素的前一個位置」的反向迭代器，因此，它表示反向迭代器的結束位置。

可以修改iterator指向位址的值。


{% highlight c++ linenos %}
reverse_iterator rend();
{% endhighlight %}

#### crend()

返回的則是指向容器「第 0 個元素的前一個位置」的反向迭代器，因此，它表示反向迭代器的結束位置。

類型可以簡化成auto，不可以修改iterator指向位址的值。

{% highlight c++ linenos %}
const_reverse_iterator crend();
{% endhighlight %}

#### 完整程式碼

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
int main() {
  vector<int> v = {1, 2, 3, 4, 5};
  //把iterator移到容器第0個元素
  vector<int>::reverse_iterator it = v.rbegin();
  //將最後一個元素的值改成7
  *it = 7;
  //iterator移到倒數第2個元素
  it++;
  //將倒數第2個元素的值改為8
  *it = 8;
  //正向疊代器
  cout << "正向 : " << endl;
  for (auto it = v.cbegin(); it != v.end(); it++) {
  cout << *it << ", ";
  }
  cout << endl;
  //反向疊代器
  cout << "反向 : " << endl;
  for (auto it = v.crbegin(); it != v.crend(); it++) {
  cout << *it << ", ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}

```
正向 : 
1, 2, 3, 8, 7, 
反向 : 
7, 8, 3, 2, 1, 
```

## 用疊代器建立容器

### vector

#### 建構子
參數為其它容器的疊代器建立vector容器

二個參數為iterator的建構子

```
vector(Iterator first, Iterator last)
```

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  //使用v1的cbegin()與v1的cend()，建立v2容器
  vector<int> v2(v1.cbegin() + 2, v1.cend() - 3);
  //遍歷v2容器所有元素
  for (auto it = v2.cbegin(); it != v2.end(); it++) {
  cout << *it << ", ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}
```
3, 4, 5, 6, 7, 
```

#### 指派

```
void assign(Iterator first, Iterator last); 
```
{% highlight c++ linenos %}
#include <iostream>
#include <vector>
#include <list>
using namespace std;
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  //使用v1的cbegin()與v1的cend()，建立v2容器
  vector<int> v2;
  v2.assign(v1.cbegin() + 2, v1.cend() - 3);
  //遍歷v2容器所有元素
  for (auto it = v2.cbegin(); it != v2.end(); it++) {
  cout << *it << ", ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}
```
3, 4, 5, 6, 7, 
```

#### 插入

返回的位址是插入元素的位址
```
iterator insert(iterator pos, const T& value); 
```

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  //在2的位址後面插入一個12
  //返回插入元素的位址(指標)
  auto iter = v1.insert(v1.begin() + 2, 12);
  cout << "插入的元素是:" << *iter << endl;
  for (auto it = v1.cbegin(); it != v1.cend(); it++) {
  cout << *it << " ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}
```
插入的元素是:12
1 2 12 3 4 5 6 7 8 9 10 
```

#### 插入區間元素

```
iterator insert(iterator pos, iterator first, iterator last); 
```
- 參數1:要插入的位址
- 參數2:其它容器的開始位址
- 參數3:其它容器的結束位址

{% highlight c++ linenos %}
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  vector<int> v2 = {11, 22, 33, 44, 55, 66, 77, 88, 99, 100};
  //在2的位址後面插入一個v2區間
  //返回第一個插入元素的位址(指標)
  auto iter = v1.insert(v1.begin() + 2, v2.begin() + 1, v2.end() - 3);
  cout << "第一個插入的元素是:" << *iter << endl;
  for (auto it = v1.cbegin(); it != v1.cend(); it++) {
  cout << *it << " ";
  }
  cout << endl;
  return 0;
}
{% endhighlight %}

```
第一個插入的元素是:22
1 2 22 33 44 55 66 77 3 4 5 6 7 8 9 10 
```

## 疊代器失靈

若在迴圈中有新增、插入、刪除的動作會導致疊代器無法移動到下一個位址。

resize(),reserve(),assign(),push_back(),pop_back(),insert(),erase()導致疊代器指向陣列元素或鏈結串列的節點位址移動，會導致iterator無法作用。


### vector刪除元素

參數都是iterator，<span class="markline">重點返回的是刪除元素的下一個位址。</span>

```
iterator erase(iterator pos); 
iterator erase(iterator first, iterator last); 
```

### 以下的程式碼會造成疊代器失靈

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
using namespace std;
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  for (vector<int>::const_iterator it = v1.begin(); it != v1.end(); it++) {
  cout << *it << " ";
  v1.erase(it);
  }
  return 0;
}
{% endhighlight %}
```
1 3 5 7 9
```


### 解決疊代器失靈

<span class="markline">把it++移除</span>，it指向刪除元素後會<span class="markline">返回下一個位址</span>，疊代器就可以正常運作。

{% highlight c++ linenos %}
int main() {
  vector<int> v1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
  for (vector<int>::const_iterator it = v1.begin(); it != v1.end(); ) {
  cout << *it << " ";
  it = v1.erase(it);
  }
  return 0;
}
{% endhighlight %}

```
1 2 3 4 5 6 7 8 9 10 
```

[1]: {% link _pages/c/dynamicMemory/memory_interval.md %}
[2]: {% link _pages/c/template/function_template.md %}