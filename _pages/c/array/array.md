---
title: 陣列
date: 2024-06-05
keywords: c++, array
---
陣列是固定大小，設定完大小，無法改變陣列大小。<br>

## 初始化值
### 方式一
{% highlight c++ linenos %}
int main() {
  int arr[3];
  arr[0] = 10;
  arr[1] = 20;
  arr[2] = 30;
  cout << "arr[0] = " << arr[0] << endl;
  cout << "arr[1] = " << arr[1] << endl;
  cout << "arr[2] = " << arr[2] << endl;
  return 0;
}
{% endhighlight %}
```
arr[0] = 10
arr[1] = 20
arr[2] = 30
```

### 方式二
```
資料型態 陣列名[] = {值1, 值2, 值3 ...};
```
以下是沒有設定陣列大小，但有給初始值。
{% highlight c++ linenos %}
int main() {
  int arr[] = {10, 20, 30};
  cout << "arr[0] = " << arr[0] << endl;
  cout << "arr[1] = " << arr[1] << endl;
  cout << "arr[2] = " << arr[2] << endl;
  return 0;
}
{% endhighlight %}
```
arr[0] = 10
arr[1] = 20
arr[2] = 30
```

### 方式三
```
資料型態 陣列名[長度] = {值1, 值2, 值3 ...};
```
若值的數量比長度小，沒有在括號{}中的值預設為0。

以下是有設定陣列大小，有給初始值。
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
## 陣列大小設定
陣列的大小設定可以用常數、運算式。
### 常數
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
//用常數設定大小
const int ARR_MAX = 10;
int main() {
  // 設定大小
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
### 運算式
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
  double d = 10;
  //使用運算式設定陣列大小
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

## 陣列中有多少元素？
sizeof(陣列名) / sizeof(陣列資料型態) = 元素個數<br>

sizeof(陣列名) 是取得這個陣列占記憶體的大小。<br>

sizeof(資料型態) <br>是取得資料資料型態占的記憶體大小，如int占4byte，double占8byte，char占1byte。<br>

## 陣列中每一個值都初始化為整數0
陣列若是全局變數，會自動初始化為預設值，如int是0、char是`\0`。<br>
但陣列若為區域變數，裡面的值預設是亂七八糟的值，不會自動設初始值。<br>
因此需要初始化陣列中的值。<br>

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

## memset陣列清空
陣列若是全局變數，會自動初始化為預設值，如int是0、char是`\0`。<br>
但陣列若為區域變數，裡面的值預設是亂七八糟的值，不會自動設初始值。<br>
因此需要初始化陣列中的值。<br>

陣列中每個元素記憶體位址的值設為0。<br>
```
void* memset(void *s, int c, size_t n);
參數1 : 陣列名
參數2 : 整數0
參數3 : 陣列記憶體大小
```
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

## 陣列無法重新指向其它記憶體位址
陣列名是記憶體位址，不是指標，不能把陣列名指向其它記憶體位址。<br>
以下程式試圖把陣列指向其它記憶體位址，都會編譯失敗。<br>
可以修改陣列中的值，但不能把陣列的起始位址指向其它記憶體位址。<br>

{% highlight c++ linenos %}
int main() {
  int arr[] = {10, 20, 30};
  int arr2[] = {40, 50, 60};
  arr = arr2;
  return 0;
}
{% endhighlight %}

{% highlight c++ linenos %}
int main() {
  char c[] = "abc";
  printf("%s \n",c);
  c = "ggg"; 
  return 0;
}
{% endhighlight %}

## memcpy() 複製陣列，記憶體內容複製
### 複製全部元素
陣列中全部的元素(來源陣列)複製到另一個大小相同的陣列(目標陣列)。

需要引入函式庫#include <string.h>
```
void* memcpy(void* dest, const void* src, size_t n)
參數1 : 目標陣列
參數2 : 來源陣列
參數3 : 陣列記憶體大小，或使用者自行定義要複製的byte。
```
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

## 複製部分元素
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

