---
title: 函式傳值與傳回值
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
二個空間的變數名稱雖然一樣，但在函式中的變數都是區域變數，存取範圍只在自己的函式中，不會影嚮其它空間的同名變數。<br>

下圖，main()函式把20傳入test()函式。<br>
![img]({{site.imgurl}}/c++/func/func_stack1.png)<br>

test()函式，把20複製給test()空間的n變數，test()空間的n變數與main()空間的n變數是不相同。
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
以下程式是把20複製給test()函式中的n變數，再進行n\+\+，但test()函式中的n是區域變數，只對test()空間有效，離開test()空間後，test()空間與test()函式中的n變數會被記憶體釋放，回到main函式後，main的n變數仍是20，因為二者為不同空間的變數，不會互相影嚮。<br>
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
```
n = 21
n = 20
```

## 複製記憶體位址
若要讓test()函式可以修改main()函式的n變數。<br>
需要使用「記憶體位址」的方式。<br>
把「記憶體位址」傳入test()函式，要使用「指標類型」，才可以把main()函式的n變數的記憶體位址「複製」到test()函式。<br>

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
透過\*取值運算子，可以取出記憶體位址中的「值」。<br>
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

## 函式傳遞值 Call by value

### 解釋1

函式會建立新的變數將引數拷貝到新的變數，將新的變數作為參數，所以在函式對參數做的任何操作，實際上是對新的變數做操作，而不是對引數做操作。

### 解釋2

參數一進入函式，函式即建立新的變數保管這些參數值，同時新變數的資料型態完全吻合參數的資料型態，這種傳遞參數的方式會先把參數複製一份，稱為傳值呼叫(call by value)。

### 解釋3 

以傳值方式傳遞參數的函式會建立一個與參數型態相同的變數，然後把參數的值複製到該變數，因此，函式不能存取原來的變數，僅能存取其拷貝值。當函式無須修改原來的變數值時，傳值的方法很實用，同時保證函式不會破壞原來的變數值。

以下程式碼是將引數x傳遞給函式setX()，將引數x拷貝到參數r，對參數r進行修改，並不會影嚮x引數。

{% highlight c++ linenos %}
//r為參數
void setX(int r){
  r = 1000;
}
int main() {
  int x = 10;
  cout << "Before x = " << x << endl;
  //x為引數
  setX(x);
  cout << "After x = " << x << endl;
  return 0;
}
{% endhighlight %}

```
Before x = 10
After x = 10
```

## 函式傳回值是值

### 解釋1
以下的程式碼，main()和getValue()都各別有存放傳回值r的記憶體位址，待getValue()的r變數傳回給main()的r變數後，getValue()的r變數的記憶體位址就會被釋放。

### 解釋2

- [記憶體配置][2]

函式會將傳回值拷貝到暫存器或stack中，然後再傳回拷貝到暫存器或stack中的值，接下來再把函式中區域變數記憶體釋放。

{% highlight c++ linenos %}
int getValue(){
  int r = 1000;
  return r;
}
int main() {
  int r = getValue();
  cout << "r = " << r << endl;
  return 0;
}
{% endhighlight %}

```
r = 1000
```

[1]: {% link _pages/c/basic/param.md %}
[2]: {% link _pages/c/dynamicMemory/memoryLayout.md %}#變數記憶體位址