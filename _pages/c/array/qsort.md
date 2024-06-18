---
title: qsort排序
date: 2024-06-15
keywords: c++, dynamic arrays, qsort
---

## qsort使用方法
```
void qsort ( void * base , size_t nitems , size_t size , int (* compar )( const void *, const void *))         
```

* 參數1，陣列名。
* 參數2，陣列大小。
* 參數3，陣列中每個元素的大小。
* 參數4，callback函式，判斷比大小的函式。

		
		int compar( const void *p1, const void *p2)
		

		* 傳回值為int整數。
			* 傳回值 > 0
				* p1會排在p2前面
			* 傳回值 == 0
				* p1與p2相等
			* 傳回值 < 0
				* p1會排在p2後面

## 程式碼
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int compare(const void* p1, const void* p2) {
    //void*指標代表接收任何資料型態的參數
    //把void*指標轉成int*指標
    int* pi1 = (int*) p1;
    int* pi2 = (int*) p2;
    //把pi1指標使用*取值運算子，取出pi1記憶體位址存放的值
    //把pi2指標使用*取值運算子，取出pi2記憶體位址存放的值
    //pi1記憶體位址的值 小於 pi2記憶體位址的值，傳回-1
    if(*pi1 < *pi2) return -1;
    //pi1記憶體位址的值 小於 pi2記憶體位址的值，傳回0
    else if(*pi1 == *pi2) return 0;
    //pi1記憶體位址的值 小於 pi2記憶體位址的值，傳回1
    //else if(*pi1 > *pi2)
    else return 1;
}
int main() {
    //宣告大小為10的整數陣列
    int arr[10] = {8,1,7,2,6,3,5,5,1,10};
    //陣列大小
    int size = sizeof(arr)/sizeof(int);
    //排序
    qsort(arr, size, sizeof(int), compare);
    for(int i = 0; i < size; i++) {
        //印出每個元素
        cout << arr[i] << ",";
    }
    cout << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
1,1,2,3,5,5,6,7,8,10,
```