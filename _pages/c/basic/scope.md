---
title: 區域全域靜態變數
date: 2024-04-18
keywords: c++, scope
---
## 區域變數與全域變數
在函式之外的變數是全域變數，整個程序都能使用它。<br>
在函式之內的變數是區域變數，存取範圍就是在該函式之中。<br>

### 函式中的區域變數
若全域變數與函式內的區域變數相同，就近原則，使用最近的區域變數。<br>
區域變數會跟著函式呼叫完畢，被記憶體回收(記憶體釋放)。<br>

以下的程式碼，func()與main()函式有區域變數n，印出區域變數n。<br>
test()沒有區域變數n，存取全域變數。<br>
{% highlight c++ linenos %}
// 全域
int n = 10;
void func() {
  // 區域n
  int n = 20;
  cout << "func n =" << n << endl;
}
void test() {
  // 函式沒定義區域變數n，使用全域n
  cout << "test n = " << n << endl;
}
int main() {
  // 區域n
  int n = 30;
  func();
  test();
  cout << "main n =" << n << endl;
  return 0;
}
{% endhighlight %}
```
func n =20
test n = 10
main n =30
```

### Block程式碼區塊中區域變數
func()函式有定義區域變數n，程式碼區塊中也有定義區域變數n。<br>
就近原則，會使用程式碼區塊中的n。<br>
{% highlight c++ linenos %}
int n = 10;
void func() {
  int n = 20;
  {
  int n = 55;
  cout << "func n =" << n << endl;
  }
}
int main() {
  func();
  return 0;
}
{% endhighlight %}
```
func n =55
```

for(int n = 1) {} 程式碼區塊，n是for程式碼區塊的區域變數。<br>
就近原則，使用for()定義的區域變數n。<br>
{% highlight c++ linenos %}
int n = 10;
void func() {
  int n = 20;
  for(int n = 1; n <10 ;n++) {
    cout << n << endl;
  }
}
int main() {
  func();
  return 0;
}
{% endhighlight %}
```
1
2
3
4
5
6
7
8
9
```

if(1) {} 程式碼區塊也是相同道理，就近原則。
{% highlight c++ linenos %}
int n = 10;
void func(int n) {
  if (1) {
    int n = 100;
    cout <<  "func n= " << n << endl;
  }
}
int main() {
  int n = 55;
  func(n);
  return 0;
}
{% endhighlight %}
```
func n= 100
```

### 函式參數
函式參數也是區域變數，存取範圍只有在函式中，不可以超出函式的範圍之外。<br>
以下程式碼func()函式有修改main()傳入的n變數，但對func而言，是把main的n變數中的值，拷貝到func()中的區域變數n的值中，修改func()函式的n變數不會影嚮main()的n變數。<br>

請參考[函式參數傳遞][1]文章。<br>
{% highlight c++ linenos %}
int n = 10;
void func(int n) {
  n++;
  cout <<  "func n= " << n << endl;
}
int main() {
  int n = 66;
  func(n);
  cout << "main n = " << n << endl;
  return 0;
}
{% endhighlight %}
```
func n= 67
main n = 66
```

## 預設值
全域變數與全域陣列都會有預設值。

|型態|預設值|
|int|0|
|char|`\0`|
|float|0.0|
|double|0.0|
|指標類型|NULL|

### 區域變數與全域變數預設值
函式內的區域變數如果不設初始值，會是亂七八糟的值。<br>
函式外的全域變數會使用預設值，如int就會是0，char就會是`\0`。<br>
所以區域變數要memset，清空亂七八糟的值。<br>
{% highlight c++ linenos %}
int n;
void func() {
  int n;
  cout << "func n = " << n << endl;
}
int main() {
  func();
  cout << "main n = " << n << endl;
  return 0;
}
{% endhighlight %}
```
func n = 32759
main n = 0
```

### 區域陣列預設值
區域陣列的值，也是亂七八糟的值，使用時要memset。<br>
全域陣列，系統會給預設值，若是int，預設值為0。<br>
{% highlight c++ linenos %}
void func() {
  int arr[5];
  int len = sizeof(arr) / sizeof(int);
  for (int i = 0; i < len; i++) {
    cout << arr[i] << endl;
  }
}
int main() {
  func();
  return 0;
}
{% endhighlight %}
```
0
0
0
0
-1074793232
```

## 區域全域變數 Memory Layout
下圖中，區域變數都在Statck。<br>
![img]({{site.imgurl}}/c++/memory.png)<br>

## 靜態變數
### 函式中靜態變數
函式中的靜態變數是共享變數，不會跟著函式被系統銷毀。<br>
func()中的靜態變數n，除了第一次呼叫func()函式會執行初始化。<br>
```
static int n = 10;
```
第二次呼叫func()函式，就不會執行初始化。<br>
靜態變數是存放在bss segment、data segment中，查上一個Memory Layout圖。<br>

{% highlight c++ linenos %}
void func() {
  static int n = 10;
  n++;
  cout << "func n = " << n << endl;
}
int main() {
  func();
  func();
  func();
  return 0;
}
{% endhighlight %}
```
func n = 11
func n = 12
func n = 13
```

### 區域變數
一對花括號`{}`包起來的就是程式碼區塊(block)或函式主體(Body)。程式碼區塊可以是if (){}或while(){}的程式碼區塊，但這篇要探討的是只有花括號`{}`包起來的程式碼區塊。

{% highlight c++ linenos %}
int main() {
  int var11 = 100;
  {
    int var11 = 10;
    cout << "inner var11 = " << var11 << endl;
  }
  cout << "outer var11 = " << var11 << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
inner var11 = 10
outer var11 = 100
```

- 第3行至第6行

在程式碼區塊中`{}`，所定義的變數都是區域變數，離開`{}`區塊就不能再讀取了，因為`{}`區塊中的區域變數已經被系統回收掉。可以想像`{}`就像是函式一樣。在函式中，函式中定義的變數都是區域變數，離開函式就不能被外部讀取區域變數，因為區域變數已經被系統回收了。

### 程式碼區塊
在`{}`區塊中，可以讀取程式碼區塊外的變數
{% highlight c++ linenos %}
int main() {
  int var11 = 100;
  int var12 = 200;
  {
    int var11 = 10;
    cout << "inner var11 = " << var11 << endl;
    //在`{}`區塊中，可以程式碼區塊外的變數
    cout << "inner var12 = " << var12 << endl;
  }
  cout << "outer var11 = " << var11 << endl;
  return 0;
}
{% endhighlight %}
```
執行結果
inner var11 = 10
inner var12 = 200
outer var11 = 100
```

### 在程式區塊中`{}`無法讀取同名的區塊外的變數
因為在`{}`區塊中，同名的區塊外變數會先被暫時隱藏，直到程式執行時離開`{}`區塊，區塊內的區域變數被系統回收掉，同名的變數就會將記憶體位置指向區塊外變數。

### 程式區塊中`{}`讀取跟區域變數同名的全域變數
使用::可以讀取全域變數。
{% highlight c++ linenos %}
//定義全域變數var11
int var11 = 100;
int main() {
  //定義區域變數var11
  int var12 = 200;
  {
    //印出區域變數var11
    int var11 = 10;
    cout << "inner var11 = " << var11 << endl;
    cout << "inner var12 = " << var12 << endl;

    //印出全域變數var11
    cout << "outer var11 = " << ::var11 << endl;
  }
  return 0;
}
{% endhighlight %}
```
執行結果
inner var11 = 10
inner var12 = 200
outer var11 = 100
```

### for程式區塊中的區域變數
變數i的有效範圍，只有在`{}`程式區塊中，離開`{}`程式區塊，區域變數i就無法在外部讀取。
{% highlight c++ linenos %}
for (int i = 0; i < 10; i++) {
  
}
{% endhighlight %}

若要變數i也可以在外部使用，可把變數i放在外部。

{% highlight c++ linenos %}
int i = 0;
for (; i < 10; i++) {  
}
cout << "i = " << i << endl;
{% endhighlight %}
```
執行結果
i = 10
```

### 虛擬碼
可以在程式區塊寫虛擬碼，進行驗證，作為之後正式的函式使用。
以下程式碼是在寫參數為指標，將指標宣告記憶體空間。

{% highlight c++ linenos %}
int main() {
  //宣告指標
  int* p = 0;//0就是nullptr 代表沒有指向任何記憶空間
  {
    int** pp = &p;//存放指標的位址，要用雙指標
    *pp = new int(3);//雙指標取值，並動態配置記憶體空間
  }

  //印出p指標的位址，去p指標的記憶體位址取值，並印出來
  cout << "p=" << p << ",*p=" << *p << endl;
  return 0;
}
{% endhighlight %}

第6行，去雙指標pp的記憶體位址取值，取出來是p的指標記憶體位址，使用new來分配動態配置記憶體空間，並且設值"3"，動態配置完成會傳回記憶體位址，由指標去存位址。


```
執行結果
p=0x60000000c000,*p=3
```
虛擬碼執行成功後，將`{}`程式碼區塊移到main()函式之外，並宣告成函式。
 
{% highlight c++ linenos %}
void initMemory(int** pp) //存放指標的位址，要用雙指標
{
  *pp = new int(3);//雙指標取值，並動態配置記憶體空間
}
int main() {
  //宣告指標
  int* p = 0;//0就是nullptr 代表沒有指向任何記憶空間
  initMemory(&p);//指標的位址傳入函式。
  cout << "p=" << p << ",*p=" << *p << endl;
  return 0;
}
{% endhighlight %}

```
執行結果
p=0x600000008030,*p=3
```

[1]: {% link _pages/c/function/callByValue.md %}