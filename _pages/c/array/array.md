---
title: 陣列
date: 2024-06-05
keywords: c++, array
---

### 初始化值

資料型態 陣列名[長度] = {值1, 值2, 值3 ...};

若值的數量比長度小，沒有在括號{}中的值預設為0。

{% highlight c++ linenos %}
int main() {
  int arr[10] = {0,1,2,3};
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
arr[0] = 0
arr[1] = 1
arr[2] = 2
arr[3] = 3
arr[4] = 0
arr[5] = 0
arr[6] = 0
arr[7] = 0
arr[8] = 0
arr[9] = 0
```

資料型態 陣列名[] = {值1, 值2, 值3 ...};

### 陣列長度

陣列的長度可以用常數、運算式。

#### 常數

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
//使用常數
const int ARR_MAX = 10;
int main() {
  int arr[ARR_MAX] = {0,1,2,3};
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
arr[0] = 0
arr[1] = 1
arr[2] = 2
arr[3] = 3
arr[4] = 0
arr[5] = 0
arr[6] = 0
arr[7] = 0
arr[8] = 0
arr[9] = 0
```

#### 運算式
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  double d = 10;
  //使用運算式設定陣列長度
  int arr[sizeof(d)/2] = {0,1,2,3};
  int size = sizeof(arr)/sizeof(int);
  cout << "arr size = " << size << endl;
  for (int i = 0; i < size; i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
arr size = 4
arr[0] = 0
arr[1] = 1
arr[2] = 2
arr[3] = 3
```


### 陣列中有多少元素？

sizeof(陣列名) / sizeof(陣列資料型態) = 元素個數

sizeof(陣列名) 是取得這個陣列占記憶體的大小。
sizeof(資料型態) 是取得資料資料型態占的記憶體大小，如int占4byte，double占8byte，char占1byte。


### 陣列中每一個值都初始化為整數0

以下二種寫法都是一樣，初始化為整數0。
```
資料型態 陣列名[長度] = {0};
資料型態 陣列名[長度] = {};
```

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  int arr[10] = {0};
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }
  
  int arr1[10] = {};
  for (int i = 0; i < sizeof(arr1)/sizeof(int); i++) {
    cout << "arr1[" << i << "] = " << arr1[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
執行結果
arr[0] = 0
arr[1] = 0
arr[2] = 0
arr[3] = 0
arr[4] = 0
arr[5] = 0
arr[6] = 0
arr[7] = 0
arr[8] = 0
arr[9] = 0
arr1[0] = 0
arr1[1] = 0
arr1[2] = 0
arr1[3] = 0
arr1[4] = 0
arr1[5] = 0
arr1[6] = 0
arr1[7] = 0
arr1[8] = 0
arr1[9] = 0
```

### memset陣列清空

陣列中每個元素記憶體位址的值設為00000000。

void* memset(void *s, int c, size_t n);

* 參數1 : 陣列名
* 參數2 : 整數0
* 參數3 : 陣列記憶體大小

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  int arr[10] = {1,2,3,4,5,6,7,8,9};
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }
  memset(arr, 0, sizeof(arr));
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }
  return 0;
}
{% endhighlight %}
```
執行結果
arr[0] = 1
arr[1] = 2
arr[2] = 3
arr[3] = 4
arr[4] = 5
arr[5] = 6
arr[6] = 7
arr[7] = 8
arr[8] = 9
arr[9] = 0
arr[0] = 0
arr[1] = 0
arr[2] = 0
arr[3] = 0
arr[4] = 0
arr[5] = 0
arr[6] = 0
arr[7] = 0
arr[8] = 0
arr[9] = 0
```

### memcpy() 複製陣列，記憶體內容複製

#### 複製全部元素
陣列中全部的元素(來源陣列)複製到另一個大小相同的陣列(目標陣列)。

需要引入函式庫#include <string.h>
void* memcpy(void* dest, const void* src, size_t n)

* 參數1 : 目標陣列
* 參數2 : 來源陣列
* 參數3 : 陣列記憶體大小，或使用者自行定義要複製的byte。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
	//來源陣列
  int arr[10] = {1,2,3,4,5,6,7,8,9,10};
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }

  //目標陣列
  int arr1[sizeof(arr) / sizeof(int)];
  memcpy(arr1, arr, sizeof(arr));
  for (int i = 0; i < sizeof(arr1)/sizeof(int); i++) {
    cout << "arr1[" << i << "] = " << arr1[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
arr[0] = 1
arr[1] = 2
arr[2] = 3
arr[3] = 4
arr[4] = 5
arr[5] = 6
arr[6] = 7
arr[7] = 8
arr[8] = 9
arr[9] = 10
arr1[0] = 1
arr1[1] = 2
arr1[2] = 3
arr1[3] = 4
arr1[4] = 5
arr1[5] = 6
arr1[6] = 7
arr1[7] = 8
arr1[8] = 9
arr1[9] = 10
```

#### 複製部分元素
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  //來源陣列
  int arr[10] = {1,2,3,4,5,6,7,8,9,10};
  for (int i = 0; i < sizeof(arr)/sizeof(int); i++) {
    cout << "arr[" << i << "] = " << arr[i] << endl;
  }

  //目標陣列
  //元素全初始化為整數0
  int arr1[sizeof(arr) / sizeof(int)] = {0};
  //只複製arr陣列8byte的元素
  //(8byte/每個元素是4byte)=2，只複製2個元素
  memcpy(arr1, arr, 8);
  for (int i = 0; i < sizeof(arr1)/sizeof(int); i++) {
    cout << "arr1[" << i << "] = " << arr1[i] << endl;
  }
  return 0;
}
{% endhighlight %}

```
arr[0] = 1
arr[1] = 2
arr[2] = 3
arr[3] = 4
arr[4] = 5
arr[5] = 6
arr[6] = 7
arr[7] = 8
arr[8] = 9
arr[9] = 10
arr1[0] = 1
arr1[1] = 2
arr1[2] = 0
arr1[3] = 0
arr1[4] = 0
arr1[5] = 0
arr1[6] = 0
arr1[7] = 0
arr1[8] = 0
arr1[9] = 0
```

