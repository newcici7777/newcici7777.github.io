---
title: 函式參數傳遞
date: 2024-07-02
keywords: c++, call by value
---
Prerequisites:

- [引數][1]

## 函式參數為基本型態
基本型態有char, short, int , long, long long, double, float。<br> 
test()函式的參數為int n，int是基本型態。
{% highlight c++ linenos %}
void test(int n) {
  cout << "n = " << n << endl;
}
int main() {
  int n = 20;
  test(n);
  return 0;
}
{% endhighlight %}

## 每個函式都是獨立空間
main()函式會有自己的空間與變數n。<br>
test()函式會有自己的空間與變數n。<br>
二個空間的變數名n雖然一樣，但不是同樣的東西，各自獨立存在各自的函式空間，存取範圍只在自己的函式中，修改main()函式的n不會更改test()函式的n，修改test()函式的n不會更改main()函式的n。<br>
就像1年1班，有一個學生叫小明。3年1班也有一個學生叫小明，但二個人只是名字一樣，但是不同人。<br>

下圖，main()函式把20傳入test()函式。<br>
![img]({{site.imgurl}}/c++/func/func_stack1.png)<br>

main()函式，把20複製給test()空間的n變數，test()空間的n變數與main()空間的n變數是不相同。
![img]({{site.imgurl}}/c++/func/func_stack2.png)<br>

test()函式呼叫cout 印出 n。
![img]({{site.imgurl}}/c++/func/func_stack3.png)<br>

test()函式全部執行完畢，在Stack中的test()函式空間會被記憶體釋放，回到main()函式中，呼叫test()的那一行。<br>
![img]({{site.imgurl}}/c++/func/func_stack4.png)<br>

{% highlight c++ linenos %}
void test(int n) {
  cout << "n = " << n << endl;
}
int main() {
  int n = 20;
  test(n);
  return 0;
}
{% endhighlight %}

印出結果:n=20<br>

## 函式中的變數
下圖中，main()函式中的n，與test()函式中的n，二者各自獨立，cout印出的都是自己函式中的n，不會跨界印出其它函式中的n。<br>
![img]({{site.imgurl}}/c++/func/func_stack8.png)<br>

## 函式不能使用其它函式的區域變數
以下會編譯不過，main()函式本身是沒有n變數，main()函式無法存取test()函式中的n變數。<br>
二者在不同的空間。<br>

{% highlight c++ linenos %}
void test(int n) {
  cout << "n = " << n << endl;
}
int main() {
  test(20);
  cout << "n = " << n << endl;
  return 0;
}
{% endhighlight %}

## 函式複製基本型態參數
呼叫函式時，若參數為基本型態，是「複製」值。<br>
以下程式是把20複製給test()函式中的n變數，再進行n\+\+，但test()函式中的變數n跟main()函式中的變數n二者是不同的，各自存在不同的空間，只對test()的n變數只對test()空間有效，離開test()空間後，test()空間與n變數會被記憶體釋放，回到main函式後，main的n變數仍是20，因為二者為不同空間的變數，修改test()函式中的n，不會跟著修改main()函式中的n。<br>

{% highlight c++ linenos %}
void test(int n) {
  n++;
  cout << "n = " << n << endl;
}
int main() {
  int n = 20;
  test(n);
  cout << "n = " << n << endl;
  return 0;
}
{% endhighlight %}

```
n = 21
n = 20
```

## 隱藏的return
當test()函式執行到最後，會有一個隱藏的return，返回呼叫test()函式的「位置」。<br>
{% highlight c++ linenos %}
void test(int n) {
  n++;
  cout << "n = " << n << endl;
  // 隱藏的return
  return;
}
int main() {
  int n = 20;
  test(n); // return返回的位置
  cout << "n = " << n << endl;
  return 0;
}
{% endhighlight %}

## 接收return
如果函式有return傳回值，就會把test()函式的值，傳回main函式，並且「要有變數去接收」傳回值，傳回值就會影嚮main()函式的變數值。<br>
![img]({{site.imgurl}}/c++/func/func_stack9.png)<br>

{% highlight c++ linenos %}
int test(int n) {
  return n + 1;
}
int main() {
  int n = 20;
  n = test(n);
  cout << "main() n = " << n << endl;
  return 0;
}
{% endhighlight %}
```
main() n = 21
```

return傳回值會取代原本`test(n)`的程式碼，直接由21取代，參考下圖。<br>
![img]({{site.imgurl}}/c++/func/func_stack10.png)<br>

如果沒有變數去接收，不會修改main()函式的變數值。<br>
執行結果n仍是20。<br>
{% highlight c++ linenos %}
using namespace std;
int test(int n) {
  return n + 1;
}
int main() {
  int n = 20;
  test(n);
  cout << "main() n = " << n << endl;
  return 0;
}
{% endhighlight %}
```
main() n = 20
```

## 複製記憶體位址
若要讓test()函式可以修改main()函式的n變數。<br>
需要使用「記憶體位址」的方式。<br>
把「記憶體位址」傳入test()函式，要使用「指標類型」，才可以把main()函式的n變數的記憶體位址「複製」到test()函式。<br>

下圖中，test()函式中的n變數，記憶體位址是0x000008，存的「值」是記憶體位址0x000001。<br>
main()函式中的n變數，記憶體位址0x000001，存的「值」是20。<br>
![img]({{site.imgurl}}/c++/func/func_stack5.png)<br>

{% highlight c++ linenos %}
void test(int* n) {
  cout << "test n address = " << n << endl;
}
int main() {
  int n = 20;
  test(&n);
  cout << "main n address = " << &n << endl;
  return 0;
}
{% endhighlight %}
```
test n address = 0x000001
main n address = 0x000001
```

## 取得其它函式中的值
test()函式中的n變數，記憶體位址是0x000008，存的「值」是記憶體位址0x000001。<br>
透過對「記憶體位址」使用\*取值運算子，可以取出記憶體位址中的「值」。<br>
`*0x000001`，取出0x000001記憶體位址存放的「值」20。<br>
![img]({{site.imgurl}}/c++/func/func_stack6.png)<br>

{% highlight c++ linenos %}
void test(int* n) {
  cout << "main() n = " << *n << endl;
}
int main() {
  int n = 20;
  test(&n);
  return 0;
}
{% endhighlight %}
```
main() n = 20
```

## 修改其它函式中的值
透過對「記憶體位址」使用\*取值運算子，可以對記憶體位址的值進行修改的操作。<br>
`*0x000001`先把記憶體的「值=20」取出來。<br>
20\+\+的意思就是把20 \+ 1，再把21存入`0x000001`記憶體位址的值。<br>
![img]({{site.imgurl}}/c++/func/func_stack7.png)<br>

## 交換
如果函式參數是基本型態，就無法修改其它函式中的變數。<br>
{% highlight c++ linenos %}
void swap(int n1, int n2) {
  int temp = n1;
  n1 = n2;
  n2 = temp;
  printf("swap() n1 = %d, n2 = %d \n", n1, n2);
}
int main() {
  int n1 = 5;
  int n2 = 10;
  swap(n1, n2);
  printf("main() n1 = %d, n2 = %d \n", n1, n2);
  return 0;
}
{% endhighlight %}
```
swap() n1 = 10, n2 = 5 
main() n1 = 5, n2 = 10
```

如果函式參數是指標(記憶體位址)，複製傳入參數為記憶體位址，就可以修改其它函式中的記憶體位址中的「值」。<br>
{% highlight c++ linenos %}
void swap(int* n1, int* n2) {
  int temp = *n1;
  *n1 = *n2;
  *n2 = temp;
  printf("swap() n1 = %d, n2 = %d \n", *n1, *n2);
}
int main() {
  int n1 = 5;
  int n2 = 10;
  swap(&n1, &n2);
  printf("main() n1 = %d, n2 = %d \n", n1, n2);
  return 0;
}
{% endhighlight %}
```
swap() n1 = 10, n2 = 5 
main() n1 = 10, n2 = 5 
```

## 函式區域變數
在函式中的變數是區域變數。<br>
若與全域變數是相同名字，函式會採取就近原則，使用函式中的區域變數。

{% highlight c++ linenos %}
int n1 = 5;
void test() {
  int n1 = 100;
  printf("test() n1 = %d \n", n1);
}
int main() {
  test();
  return 0;
}
{% endhighlight %}
```
test() n1 = 100 
```

函式中的區域變數要設初始值。<br>
「全域」變數會根據基本型態設定對映的初始值，如:int是0，char是`\0`，「全域」變數是整個程式都可以共享，可以放在標頭檔.h，但只能被include一次。<br>
函式中的區域變數不會設定對映的初始值，如果不設初始值，會有亂七八糟的值，甚至造成程式執行失敗。<br>

{% highlight c++ linenos %}
// 全域變數，沒設初始值，預設為0
int n2;
void test() {
  // 區域變數沒設初始值，預設為亂七八糟的值。
  int n1;
  printf("test() n1 = %d \n", n1);
  printf("test() n2 = %d \n", n2);
}
int main() {
  test();
  return 0;
}
{% endhighlight %}
```
test() n1 = 32759 
test() n2 = 0 
```

從執行結果，可以知道區域變數與全域變數的預設值有很大不同。



[1]: {% link _pages/c/basic/param.md %}
[2]: {% link _pages/c/dynamicMemory/memoryLayout.md %}#變數記憶體位址