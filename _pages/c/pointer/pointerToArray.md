---
title: 一維指標陣列
date: 2024-06-05
keywords: c++, pointer to pointer
---
### 陣列的記憶體位址是連續的。
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5];
    cout << "array[0]地址 = " << (long long) &array[0] << endl;
    cout << "array[1]地址 = " << (long long) &array[1] << endl;
    cout << "array[2]地址 = " << (long long) &array[2] << endl;
    cout << "array[3]地址 = " << (long long) &array[3] << endl;
    cout << "array[4]地址 = " << (long long) &array[4] << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
array[0]地址 = 140702053823568
array[1]地址 = 140702053823572
array[2]地址 = 140702053823576
array[3]地址 = 140702053823580
array[4]地址 = 140702053823584
```
從以上的執行結果可以知道每個位址差距4byte，而且是連續的。

### 陣列名就是陣列第0個元素的記憶體位址

C++將陣列名視為陣列第0個元素的記憶體位址。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5];
    cout << "陣列名 = " << array << endl;
    cout << "陣列名地址 = " << &array << endl;
    cout << "array[0]地址 = " << &array[0] << endl;
    return 0;
}
{% endhighlight %}

```
執行結果
陣列名 = 0x7ff7bfeff450
陣列名地址 = 0x7ff7bfeff450
array[0]地址 = 0x7ff7bfeff450
```
從以上的執行結果可以知道印出`陣列名`與使用`&取址運算子+陣列名`與印出`陣列第0個元素位址`的結果是一樣的。

### 陣列運算

陣列名 + 1，位址移動的範圍取決於陣列的資料型態。

{% highlight c++ linenos %}
    int arr[] = {10, 100, 200};
    //印出arr[0]的地址
    cout << "arr地址=" << arr << endl;
    //印出arr[0]的地址
    cout << "arr+0地址=" <<  arr+0 << endl;
    //印出arr[1]的地址
    cout << "arr+1地址=" <<  arr+1 << endl;
    //印出arr[2]的地址
    cout << "arr+2地址=" <<  arr+2 << endl;
{% endhighlight %}

```
執行結果
arr地址=0x7ff7bfeff45c
arr+0地址=0x7ff7bfeff45c
arr+1地址=0x7ff7bfeff460
arr+2地址=0x7ff7bfeff464
```

從以上的執行結果可以知道每次陣列名 + n，移動的位址n \* 4byte(陣列的資料型態為int)。

陣列第n個元素的記憶體位址 = 陣列名 + n

### 陣列名[索引] 等同 *(陣列名 + 索引)

透過\*取值運算子，可以取出記憶體位址存放的內容。

C++編譯器將陣列名[索引] 解釋為 *(陣列名 + 索引)

arr[0]與\*(arr \+ 0)是相同的意思，都是對陣列第0個元素的記憶體位址取出存放的內容。

{% highlight c++ linenos %}
    int arr[] = {10, 100, 200};
    //印出arr[0]的地址
    cout << "arr地址=" << arr << endl;
    //印出arr[0]的地址
    cout << "arr+0地址=" <<  arr+0 << endl;
    //印出arr[1]的地址
    cout << "arr+1地址=" <<  arr+1 << endl;
    //印出arr[2]的地址
    cout << "arr+2地址=" <<  arr+2 << endl;
    //印出arr[0]的值
    cout << "arr值=" << *(arr) << endl;
    //印出arr[0]的值
    cout << "arr+0值=" << *(arr+0) << endl;
    //印出arr[1]的值
    cout << "arr+1值=" <<  *(arr+1) << endl;
    //印出arr[2]的值
    cout << "arr+2值=" <<  *(arr+2) << endl;
{% endhighlight %}

```
執行結果
arr地址=0x7ff7bfeff45c
arr+0地址=0x7ff7bfeff45c
arr+1地址=0x7ff7bfeff460
arr+2地址=0x7ff7bfeff464
arr值=10
arr+0值=10
arr+1值=100
arr+2值=200
```

### 指標存陣列名

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5];
    cout << "陣列名 = " << array << endl;
    cout << "陣列名地址 = " << &array << endl;
    cout << "array[0]地址 = " << &array[0] << endl;
    
    int* p = array;
    cout << "p指標內容 = " << p << endl;
    return 0;
}
{% endhighlight %}


第9行，將陣列名(也就是陣列第0個元素的記憶體位址)指定至指標變數p。

第10行，印出指標變數p，也就是印出第0個元素記憶體位址。

```
執行結果
陣列名 = 0x7ff7bfeff450
陣列名地址 = 0x7ff7bfeff450
array[0]地址 = 0x7ff7bfeff450
p指標內容 = 0x7ff7bfeff450
```
p指標內容為陣列名的地址。

### 指標陣列運算
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5];
    cout << "陣列名 = " << array << endl;
    cout << "陣列名地址 = " << &array << endl;
    cout << "array[0]地址 = " << &array[0] << endl;
    cout << "array[1]地址 = " << &array[1] << endl;
    cout << "array[2]地址 = " << &array[2] << endl;
    cout << "array[3]地址 = " << &array[3] << endl;

    int* p = array;
    cout << "p指標內容 = " << p << endl;
    cout << "p指標+0 = " << p + 0 << endl;
    cout << "p指標+1 = " << p + 1 << endl;
    cout << "p指標+2 = " << p + 2 << endl;
    cout << "p指標+3 = " << p + 3 << endl;
    return 0;
}
{% endhighlight %}

第12行，將陣列名(也就是陣列第0個元素的記憶體位址)指定至指標變數p。

第13行，指標變數p，印出第0個元素記憶體位址。

第14行，指標變數p + 0，印出第0個元素記憶體位址。

第15行，指標變數p + 1，印出第1個元素記憶體位址。

第16行，指標變數p + 2，印出第2個元素記憶體位址。

第17行，指標變數p + 3，印出第3個元素記憶體位址。

第18行，指標變數p + 4，印出第4個元素記憶體位址。

```
執行結果
陣列名 = 0x7ff7bfeff450
陣列名地址 = 0x7ff7bfeff450
array[0]地址 = 0x7ff7bfeff450
array[1]地址 = 0x7ff7bfeff454
array[2]地址 = 0x7ff7bfeff458
array[3]地址 = 0x7ff7bfeff45c
p指標內容 = 0x7ff7bfeff450
p指標+0 = 0x7ff7bfeff450
p指標+1 = 0x7ff7bfeff454
p指標+2 = 0x7ff7bfeff458
p指標+3 = 0x7ff7bfeff45c
```
由以上的結果可知，指標 + 1 與 &array[1]的結果是一樣的。

### 指標陣列運算取值

透過\*取值運算子，可以取出記憶體位址存放的內容。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5] = {1,2,3,4,5};
    cout << "array[0] = " << array[0] << endl;
    cout << "array[1] = " << array[1] << endl;
    cout << "array[2] = " << array[2] << endl;
    cout << "array[3] = " << array[3] << endl;
    cout << "array[4] = " << array[4] << endl;
    
    int* p = array;
    cout << "*p指標 = " << *p << endl;
    cout << "*p指標+0 = " << *(p + 0) << endl;
    cout << "*p指標+1 = " << *(p + 1) << endl;
    cout << "*p指標+2 = " << *(p + 2) << endl;
    cout << "*p指標+3 = " << *(p + 3) << endl;
    cout << "*p指標+3 = " << *(p + 4) << endl;
    return 0;
}
{% endhighlight %}

第11行，將陣列名(也就是陣列第0個元素的記憶體位址)指定至指標變數p。

第12行，指標變數p使用取值運算子，取出第0個元素記憶體位址中的值。

第13行，指標變數p + 0使用取值運算子，取出第0個元素記憶體位址中的值。

第14行，指標變數p + 1使用取值運算子，取出第1個元素記憶體位址中的值。

第15行，指標變數p + 2使用取值運算子，取出第2個元素記憶體位址中的值。

第16行，指標變數p + 3使用取值運算子，取出第3個元素記憶體位址中的值。

第17行，指標變數p + 4使用取值運算子，取出第4個元素記憶體位址中的值。

```
執行結果
array[0] = 1
array[1] = 2
array[2] = 3
array[3] = 4
array[4] = 5
*p指標 = 1
*p指標+0 = 1
*p指標+1 = 2
*p指標+2 = 3
*p指標+3 = 4
*p指標+3 = 5
```

### *(指標 + 索引) 等同 指標[索引]

C++編譯器將指標[索引] 解釋為 *(指標 + 索引)

指標就是存放記憶體位址，陣列名就是陣列第0個元素的記憶體位址，陣列名也是記憶體位址，所以陣列名[索引]等同記憶體位址[索引]，而指標是記憶體位址，等同記憶體位址[索引]，二者是相同。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5] = {1,2,3,4,5};
    cout << "array[0] = " << array[0] << endl;
    cout << "array[1] = " << array[1] << endl;
    cout << "array[2] = " << array[2] << endl;
    cout << "array[3] = " << array[3] << endl;
    cout << "array[4] = " << array[4] << endl;
    
    int* p = array;
    cout << "*p指標 = " << *p << endl;
    cout << "*p指標+0 = " << *(p + 0) << endl;
    cout << "*p指標+1 = " << *(p + 1) << endl;
    cout << "*p指標+2 = " << *(p + 2) << endl;
    cout << "*p指標+3 = " << *(p + 3) << endl;
    cout << "*p指標+3 = " << *(p + 4) << endl;

    cout << "p指標[索引0] = " << p[0] << endl;
    cout << "p指標[索引1] = " << p[1] << endl;
    cout << "p指標[索引2] = " << p[2] << endl;
    cout << "p指標[索引3] = " << p[3] << endl;
    cout << "p指標[索引4] = " << p[4] << endl;
    return 0;
}
{% endhighlight %}

第19-23行，使用指標[索引]的方式，把記憶體位址存放的值取出來。

```
執行結果
array[0] = 1
array[1] = 2
array[2] = 3
array[3] = 4
array[4] = 5
*p指標 = 1
*p指標+0 = 1
*p指標+1 = 2
*p指標+2 = 3
*p指標+3 = 4
*p指標+3 = 5
p指標[索引0] = 1
p指標[索引1] = 2
p指標[索引2] = 3
p指標[索引3] = 4
p指標[索引4] = 5
```

### 記憶體位址[索引] 等同 *(記憶體位址 + 索引)
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5] = {1,2,3,4,5};
    cout << "array[0] = " << array[0] << endl;
    cout << "array[1] = " << array[1] << endl;
    cout << "array[2] = " << array[2] << endl;
    cout << "array[3] = " << array[3] << endl;
    cout << "array[4] = " << array[4] << endl;
    
    cout << "記憶體位址 = " << &array[2] << endl;
    cout << "*記憶體位址 = " << *(&array[2]) << endl;
    cout << "*記憶體位址+0 = " << *(&array[2] + 0) << endl;
    cout << "*記憶體位址+1 = " << *(&array[2] + 1) << endl;
    cout << "*記憶體位址+2 = " << *(&array[2] + 2) << endl;

    cout << "記憶體位址[索引0] = " << (&array[2])[0] << endl;
    cout << "記憶體位址[索引1] = " << (&array[2])[1] << endl;
    cout << "記憶體位址[索引2] = " << (&array[2])[2] << endl;

    int* p = &array[2];
    cout << "p指標[索引0] = " << p[0] << endl;
    cout << "p指標[索引1] = " << p[1] << endl;
    cout << "p指標[索引2] = " << p[2] << endl;
    return 0;
}
{% endhighlight %}

第11行，印出陣列名[索引2]的記憶體位址。

第12行，&陣列名[索引2]，代表取出陣列名[索引2]的記憶體位址，使用\*取值運算子把記憶體位址存的值取出來。

第13行，&陣列名[索引2]+0，記憶體位址往後移動0byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並使用\*取值運算子把記憶體位址存的值取出來。

第14行，&陣列名[索引2]+1，記憶體位址往後移動4byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並使用\*取值運算子把記憶體位址存的值取出來。

第15行，&陣列名[索引2]+2，記憶體位址往後移動8byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並使用\*取值運算子把記憶體位址存的值取出來。

第17行，(&陣列名[索引2])[0]，記憶體位址往後移動0byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並使用\*取值運算子把記憶體位址存的值取出來。

第18行，(&陣列名[索引2])[1]，記憶體位址往後移動4byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並使用\*取值運算子把記憶體位址存的值取出來。

第19行，(&陣列名[索引2])[2]，記憶體位址往後移動8byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並使用\*取值運算子把記憶體位址存的值取出來。

第21行，取出陣列名[索引2]的記憶體位址指派給指標變數p。

第22行，指標[索引0]，記憶體位址往後移動0byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並把記憶體位址存的值取出來。

第23行，指標[索引1]，記憶體位址往後移動4byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並把記憶體位址存的值取出來。

第24行，指標[索引2]，記憶體位址往後移動8byte(取決於記憶體位址的資料型態，目前的資料型態是int)，並把記憶體位址存的值取出來。

```
執行結果
array[0] = 1
array[1] = 2
array[2] = 3
array[3] = 4
array[4] = 5
*p指標 = 3
*p指標+0 = 3
*p指標+1 = 4
*p指標+2 = 5
記憶體位址[索引0] = 3
記憶體位址[索引1] = 4
記憶體位址[索引2] = 5
p指標[索引0] = 3
p指標[索引1] = 4
p指標[索引2] = 5
```
### 指標陣列與迴圈

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5] = {1,2,3,4,5};
    int size = sizeof(array) / sizeof(int);
    for(int i = 0; i < size; i++) {
        cout << "array["<< i << "] 記憶體位址 = " << &array[i] << endl;
        cout << "array+"<< i << " 記憶體位址 = " << array + i << endl;
        cout << "array["<< i << "] 的值 = " << array[i] << endl;
        cout << "*(array+"<< i << ") 的值 = " << *(array + i) << endl;
    }
    
    int* p = array;
    for(int i = 0; i < size; i++) {
        cout << "p + "<< i << " 記憶體位址 = " << p + i << endl;
        cout << "*(p + "<< i << ") 的值 = " << *(p + i) << endl;
    }
    return 0;
}
{% endhighlight %}

第7行，印出array[i]記憶體位址。

第8行，印出array + i記憶體位址。

第9行，印出array[i]的值。

第10行，印出*(array + i)的值。

第13行，將陣列名(也就是陣列第0個元素的記憶體位址)指定至指標變數p。

第15行，印出p + i的記憶體位址。

第16行，印出*(p + i)記憶體位址存放的值。

```
執行結果
array[0] 記憶體位址 = 0x7ff7bfeff450
array+0 記憶體位址 = 0x7ff7bfeff450
array[0] 的值 = 1
*(array+0) 的值 = 1
array[1] 記憶體位址 = 0x7ff7bfeff454
array+1 記憶體位址 = 0x7ff7bfeff454
array[1] 的值 = 2
*(array+1) 的值 = 2
array[2] 記憶體位址 = 0x7ff7bfeff458
array+2 記憶體位址 = 0x7ff7bfeff458
array[2] 的值 = 3
*(array+2) 的值 = 3
array[3] 記憶體位址 = 0x7ff7bfeff45c
array+3 記憶體位址 = 0x7ff7bfeff45c
array[3] 的值 = 4
*(array+3) 的值 = 4
array[4] 記憶體位址 = 0x7ff7bfeff460
array+4 記憶體位址 = 0x7ff7bfeff460
array[4] 的值 = 5
*(array+4) 的值 = 5
p + 0 記憶體位址 = 0x7ff7bfeff450
*(p + 0) 的值 = 1
p + 1 記憶體位址 = 0x7ff7bfeff454
*(p + 1) 的值 = 2
p + 2 記憶體位址 = 0x7ff7bfeff458
*(p + 2) 的值 = 3
p + 3 記憶體位址 = 0x7ff7bfeff45c
*(p + 3) 的值 = 4
p + 4 記憶體位址 = 0x7ff7bfeff460
*(p + 4) 的值 = 5
```

### 指標陣列遞增遞減

#### 指標陣列++
```
    int array[5] = {1,2,3,4,5};
    int* p = array;
```
將陣列第0個元素的記憶體位址指定至指標變數p。

|指標運算|記憶體位址|記憶體位址存放的值|
|:---:|:---:|:---:|
|p + 0| 140702053823568 | 1 |
|p + 1| 140702053823572 | 2 |
|p + 2| 140702053823576 | 3 |
|p + 3| 140702053823580 | 4 |
|p + 4| 140702053823584 | 5 |

指標變數p指向的是一個整數資料型態(4byte)的地址，每一次p + 1，指標變數p就會移動4byte。

p++也就是等於 p = p + 1
{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    int array[5] = {1,2,3,4,5};
    int size = sizeof(array) / sizeof(int);
    int* p = array;
    for(int i = 0; i < size; i++) {
        cout << "p + "<< i << " 記憶體位址 = " << (long long)p  << endl;
        cout << "p + "<< i << " 的值 = " << *p << endl;
        p++;
    }
    return 0;
}
{% endhighlight %}

第6行，將陣列名(也就是陣列第0個元素的記憶體位址)指定至指標變數p。

第8行，印出指標變數p的內容(指標存放的內容是記憶體位址)。

第9行，印出指標變數p記憶體位址存放的值。

第10行，p = p + 1，指標變數移動4byte，將移動後的記憶體位址指派給指標變數p。

```
執行結果
p + 0 記憶體位址 = 140702053823568
p + 0 的值 = 1
p + 1 記憶體位址 = 140702053823572
p + 1 的值 = 2
p + 2 記憶體位址 = 140702053823576
p + 2 的值 = 3
p + 3 記憶體位址 = 140702053823580
p + 3 的值 = 4
p + 4 記憶體位址 = 140702053823584
p + 4 的值 = 5
```

#### \*ptr++

指標先取出記憶體位址存放的值，再往下一個位址移動。
{% highlight c++ linenos %}
    //宣告一個陣列
    int arr[] = {10, 100, 200};
    //將arr的第一個值(10)的位址傳給ptr指標
    //ptr是整數型態4byte的指標
    int *ptr = arr;
    for(int i = 0; i < 3; i++) {
        printf("值= %d\n",*ptr++);
    }
{% endhighlight %}

```
執行結果
值= 10
值= 100
值= 200
```

#### \*++ptr

指標先往下一個記憶體位址移動，再取出記憶體位址存放的值。
{% highlight c++ linenos %}
    //宣告一個陣列
    int arr[] = {10, 100, 200};
    //將arr的第一個值(10)的位址傳給ptr指標
    //ptr是整數型態4byte的指標
    int *ptr = arr;
    for(int i = 0; i < 3; i++) {
        printf("值= %d\n",*++ptr);
    }
{% endhighlight %}

```
執行結果
值= 100
值= 200
值= 0
```

#### ptr\-\-指標遞減

將指標指向前一個記憶體位址

{% highlight c++ linenos %}
    int arr[] = {10, 100, 200};
    //取得陣列中最後一個值的記憶體位址
    int *ptr = &arr[2];
    for(int i = 3; i > 0; i--) {
        printf("arr[%d]:記憶體位址= %#x\n",i ,ptr);
        printf("arr[%d]:值= %d\n",i ,*ptr);
        //將指標指向前一個記憶體位址
        ptr--;
    }    
{% endhighlight %}    

```
執行結果
arr[3]:記憶體位址= 0xbfdff37c
arr[3]:值= 200
arr[2]:記憶體位址= 0xbfdff378
arr[2]:值= 100
arr[1]:記憶體位址= 0xbfdff374
arr[1]:值= 10
```

### 陣列名是常數，不可修改

指標變數p可以設為其它記憶體位址。

```
p = p + 1;
p++;
```

陣列名無法再設為其它記憶體位址，以下的語法無法編譯成功。

```
array = array + 1;
array++;
```

### 記憶體位址轉型

以下程式碼將字元陣列轉型成整數陣列。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int main() {
    //宣告字元陣列 大小20byte
    char array[20];
    //將字元陣列的記憶體空間轉型成整數資料型態的記憶體空間
    //把轉型成整數型態的陣列[0]位址，指派給整數型態的指標變數p
    int* p = (int*)array;
    //sizeof(陣列名)取得陣列全部記憶體大小為20byte
    //再除以整數資料型態的大小sizeof(int) = 4byte
    int size = sizeof(array)/sizeof(int);
    cout << "size = " << size << endl;
    for(int i = 0; i < size; i++) {
        p[i] = i;//修改記憶體位址中存放的值
        //上面的寫法與下面的寫法是相同意思
        //*(p + i) = i;
    }
    for(int i = 0; i < size; i++) {
        //印出記憶體位址存放的值
        cout <<"*(p + "<< i <<") = " << p[i] << endl;
    }
    return 0;
}
{% endhighlight %}

第8行，將字元陣列轉型成整數陣列。

第11行，計算陣列元素有幾個。

第14行，修改每個陣列元素的值。

第20行，印出陣列元素的值。

```
執行結果
*(p + 0) = 0
*(p + 1) = 1
*(p + 2) = 2
*(p + 3) = 3
*(p + 4) = 4
```

### 函式的參數為一維指標陣列

參數的寫法有下面二種，因為傳入的參數是指標，對指標用sizeof(指標)，只會得到8byte，所以一定要傳入陣列的長度。
```
void func(int* arr, int len);
```
* 參數1，指標變數名arr。
* 參數2，陣列長度。

```
void func(int arr[], int len);
```
* 參數1，指標變數名arr[]，使用陣列表示法不代表是陣列，而是指標。
* 參數2，陣列長度。


以下程式碼為傳陣列給函式的寫法。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
void func(int arr[], int len) {
    cout << "arr指標大小 = " << sizeof(arr) << endl;
    for(int i = 0; i < len; i++) {
        cout << "arr[" << i << "] = " << *(arr + i) << endl;
    }
}
int main() {
    int array[] = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15};
    func(array,sizeof(array)/sizeof(int));
}
{% endhighlight %}

第6行，使用*(陣列名 + 索引)印出陣列元素，也可以使用陣列名[索引]的方式印出陣列元素。

第11行，參數1傳入陣列名，陣列名為陣列第0個元素的記憶體位址，參數2傳入陣列大小。

```
執行結果
arr指標大小 = 8
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
arr[10] = 11
arr[11] = 12
arr[12] = 13
arr[13] = 14
arr[14] = 15
```
