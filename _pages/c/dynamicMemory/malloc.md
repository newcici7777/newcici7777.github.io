---
title: malloc
date: 2025-03-13
keywords: c, malloc
---
Prerequisites:

- [void\* 指標][3]

## 語法
{% highlight c++ linenos %}
void* malloc(size_t size)
{% endhighlight %}

### 傳回值型態
由於malloc傳回的類型是void\*指標位址，void\*可以強轉成任何型態。<br>

傳回值是void\*要[強制轉型][1]。

傳回記憶體空間的起始位址。<br>

例:建立5個int空間，int空間占4byte，也就是建立5 \* 4byte = 20byte。<br>

下圖中，p變數指向第1個int空間的起始位址，總共建立5個int空間。<br>
![img]({{site.imgurl}}/c++/malloc1.png)<br>

{% highlight c++ linenos %}
  int len = 5;
  int* p = (int * )malloc(len * sizeof(int));
{% endhighlight %}

其它範例:<br>
{% highlight c++ linenos %}
char* p1 = (char* )malloc(10);  // 10byte
int* p2 = (int* )malloc(1 * 1024 * 1024);  // 1byte * 1024 = 1kb ->1kb * 1024=1mb
{% endhighlight %}

### 參數size_t
size_t是無正負號，unsigned int整數。<br>

參數`size_t size`代表設定空間大小，單位是byte。

也可以這樣定義
{% highlight c++ linenos %}
// int 4byte * 10 = 40 byte
// 申請40byte的記憶體空間
int* num = (int* )malloc(sizeof(int) * 10);
{% endhighlight %}

char是1byte，就不用寫sizeof(char)。<br>
{% highlight c++ linenos %}
char* name = (char* )malloc(100);
{% endhighlight %}

### 初始化memset
-[memset][2]
把指標所指向的記憶體內容，初始化。<br>
以下是把p指標所指向的記憶體空間，全變成0。<br>

c++的0，就是nullptr、NULL。<br>

各種型態的預設值都是0，char的`\0`，也是0，對映ASCII CODE碼`\0`就是整數0。

|型態|預設值|
|int|0|
|char|`\0`|
|float|0.0|
|double|0.0|
|指標類型|NULL|

{% highlight c++ linenos %}
size_t size = 1 * 1024 * 1024;
void* p = malloc(size);
memset(p, 0, size);  // 初始化都為0
{% endhighlight %}

### 記憶體回收
所謂的記憶體回收，是指這個空間已經沒有人使用了，還給系統，並<span class="markline">沒有清空</span>。<br>
只是告訴系統，這塊空間已經沒人用。<br>
下一次系統可以分配變數使用這個記憶體空間。<br> 
{% highlight c++ linenos %}
size_t size = 1 * 1024 * 1024;
void* p = malloc(size);
memset(p, 0, size);  // 初始化都為0
free(p);
{% endhighlight %}

下面的程式，func()函式執行完後，區域變數n已經被系統回收。<br>
main()函式，試圖把func()函式中的n變數取出來，但印出的值已經不是100，因為原本n的記憶體空間已經被系統拿去放"test test"的內容，所謂的回收，就是告訴系統這塊記憶體空間不使用，可以拿來放其它的值。<br>
{% highlight c++ linenos %}
int* func() {
  int n = 100;
  return &n;
}
int main() {
  int* p = func();
  int n;
  printf("test test\n");
  n = * p;
  printf("%d\n",n);
  return 0;
}
{% endhighlight %}
```
test test
-610123577
```
## 指標p設為nullptr
記憶體回收完畢，要把p指標指向nullptr。<br>
不能讓p指標指向「已回收」的記憶體空間。<br>
p也可以指向0。<br>
p也可以指向NULL。<br>
都是相同的意思。<br>
{% highlight c++ linenos %}
free(p);  
p = nullptr;
// p = 0;  
// p = NULL    
{% endhighlight %}

## 建立陣列
以下程式碼建立大小為5的整數(4 byte)陣列。<br>
sizeof(int)為4byte。<br>
大小 = 5 \* 4byte = 20 byte <br>

空間建立完後，會有一個記憶體空間，裡面有5個空間，每個空間都是4byte。<br>
p指標指向記憶體空間的起始位址。<br>
![img]({{site.imgurl}}/c++/malloc1.png)<br>

### 指標移動
因為是`int*`的指標，所以每一次移動都是移動4byte。<br>

p是記憶體位址，p = 0x1000，\+ 1 代表記憶體位址 \+ 4 byte。<br>
p + 0 = 0x1000 + 0byte = 0x1000 <br>
p + 1 = 0x1000 + 4byte = 0x1004 <br>
p + 2 = 0x1000 + 8byte = 0x1008 <br>
p + 3 = 0x1000 + 12byte = 0x100C <br>
p + 4 = 0x1000 + 16byte = 0x1010 <br>
![img]({{site.imgurl}}/c++/malloc2.png)<br>

p\+\+ 就是 p = p \+ 1，\+ 1 代表記憶體位址 \+ 4 byte。<br>

但這邊有用等於，代表每移動1次，p變數也會修改。<br>

因為p會修改，所以下面程式使用p2記錄p指標一開始的起始位址。<br>

{% highlight c++ linenos %}
int main() {
  int len = 5;
  // p 指向起始位址
  int* p = (int * )malloc(len * sizeof(int));
  // 記錄p指標一開始的起始位址
  int* p2 = p;
  for (int i = 0; i < len; i++) {
    // 記憶體位址的值，指派為i變數
    * p = i;
    // p = p + 1，每移動一次，p的位址也會更改
    p++;
  }
  for (int i = 0; i < len; i++) {
    // 使用p2印出值
    cout << * (p2 + i) << ", ";
  }
  cout << endl;
  // 記憶體位址回收
  free(p);
  // p指標設為null
  p = nullptr;  
  return 0;
}
{% endhighlight %}
```
0, 1, 2, 3, 4, 
```

[1]: {% link _pages/c/conversion/type_conversion.md %}
[2]: {% link _pages/c/array/array.md %}#memset陣列清空
[3]: {% link _pages/c/pointer/pointerVoid.md %}