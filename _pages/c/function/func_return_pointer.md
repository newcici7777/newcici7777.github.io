---
title: 函式傳回指標
date: 2025-09-08
keywords: c++, function return pointer 
---
## 傳回值不能是區域變數的位址
傳回的記憶體位址，不能是函式中區域變數的記憶體位址，因為函式呼叫完畢後，函式記憶體空間與區域變數的記憶體會被系統回收。<br>
注意！這邊說的記憶體回收並不是值被清空，而是多出一塊空餘記憶體空間，可以給其它變數或字串或建立物件使用。<br>

下面的程式碼，func()函式傳回區域變數n的記憶體位址。<br>
main()函式中，p指標變數接收函式回傳的記憶體位址。<br>
印出\"test test\"。<br>
接下來p指標(記憶體位址)使用\*取值運算子，把記憶體位址的值取出來，請問輸出結果會是什麼？<br>
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
輸出結果並非100，而是-610123577。

## 使用static靜態變數
靜態變數不會隨著函式呼叫完畢，記憶體位址被系統回收。<br>
靜態變數會存在data segment。<br>
把func()函式中的n變數設為static。<br>
{% highlight c++ linenos %}
int* func() {
  static int n = 100;
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
100
```
輸出結果是印出100，而不是亂七八糟的數字。

## 記憶體位址由參數傳入
函式不負責建立記憶體位址空間，比如下面的程式，由main()函式建立變數記憶體空間，再由參數傳入func()函式，c1、c2的生命周期會隨著main()函式執行完畢一起結束，而不是隨著func()函式而結束。
{% highlight c++ linenos %}
char* func(char* c1, char* c2) {
  if (strlen(c1) > strlen(c2))
    return c1;
  else
    return c2;
}
int main() {
  char c1[100] = "Hello World!";
  char c2[100] = "ABCDEF";
  char* c = func(c1, c2);
  cout << c << endl;
  return 0;
}
{% endhighlight %}
```
Hello World!
```

## 使用動態配置記憶體空間
動態配置記憶體空間，儲存的位址是在Heap，不會隨著函式呼叫完畢，記憶體位址被系統回收。<br>
但動態配置記憶體需要自行delete 指標，並把指標設為nullptr，因為c++不會在離開main()函式後，自動清除動態配置記憶體。<br>
{% highlight c++ linenos %}
int* func() {
  int* n = new int(50);
  return n;
}
int main() {
  int* p = func();
  int n;
  printf("test test\n");
  n = * p;
  printf("%d\n",n);
  delete p;
  p = nullptr;
  return 0;
}
{% endhighlight %}
```
test test
50
```

## 使用全域變數
隨著整個程式結束，全域變數才會被記憶體回收。

## 傳回陣列記憶體位址
以下程式碼，在func()函式建立區域變數arr，是錯誤的。
{% highlight c++ linenos %}
int* func() {
  int arr[5] = {1, 2, 3, 0, 0};
  return arr;
}
int main() {
  int* p = func();
  return 0;
}
{% endhighlight %}

正確作法如下:<br>

### 使用動態配置記憶體
{% highlight c++ linenos %}
#include <iostream>
using namespace::std;
int* func(int size) {
  int* arr = new int[size] {1, 2, 3, 0, 0};
  return arr;
}
int main() {
  int size = 5;
  int* p = func(size);
  // 使用original 記錄最初的陣列位址，後面要刪除，因為p是會移動
  int* original = p;
  for (int i = 0; i < size; i++) {
    cout << * p << ", ";
    p++;
  }
  cout << endl;
  // 使用original 記錄最初的陣列位址，後面要刪除，因為p是會移動
  delete[] original;
  original = nullptr;
  return 0;
}
{% endhighlight %}
```
1, 2, 3, 0, 0, 
```

### 使用static
使用靜態陣列有些麻煩，因為static是編譯時就要知道陣列大小，所以一定要用`#define`來設定大小。
{% highlight c++ linenos %}
#define ARR_SIZE 5
int* func() {
  static int arr[ARR_SIZE]= {1, 2, 3, 0, 0};
  return arr;
}
int main() {
  int* p = func();
  for (int i = 0; i < ARR_SIZE; i++) {
    cout << * p << ", ";
    p++;
  }
  cout << endl;
  return 0;
}
{% endhighlight %}
```
1, 2, 3, 0, 0, 
```

## 回傳物件記憶體位址
以下程式碼使用動態分配記憶體，但要記得delete。
{% highlight c++ linenos %}
class Student {
public:
  const char* name;
  int id;
  const char* addr;
};
Student* createStudent() {
  return new Student;
}
int main() {
  Student* p = createStudent();
  p->name = "Mary";
  p->id = 10;
  p->addr = "Taoyuan";
  printf("name = %s , id = %d, addr= %s \n", p->name, p->id, p->addr);
  delete p;
  p = nullptr;
  return 0;
}
{% endhighlight %}
```
name = Mary , id = 10, addr= Taoyuan 
```

以下是錯誤的方式，因為student是區域變數，會隨著函式執行完畢，記憶體被回收。
{% highlight c++ linenos %}
Student* createStudent() {
  Student student;
  return &student;
}
{% endhighlight %}

## 字串回傳記憶體位址
以下是錯誤，在Stack中建立str字串陣列，函式func執行完畢，記憶體位址會被系統回收。<br>
{% highlight c++ linenos %}
char* func() {
  char str[100] = "Hello World";
  return str;
}
int main() {
  char* str = func();
  cout << str << endl;
  return 0;
}
{% endhighlight %}

以下是正確方式。<br>
可使用RODATA區塊，也就是字串常數，不會因為函式結束而被記憶體回收。<br>
{% highlight c++ linenos %}
const char* func() {
  const char* str = "Hello World";
  return str;
}
int main() {
  const char* str = func();
  cout << str << endl;
  return 0;
}
{% endhighlight %}
```
Hello World
```

以上程式碼可以簡化如下:
{% highlight c++ linenos %}
const char* func() {
  return "Hello World";
}
int main() {
  const char* str = func();
  cout << str << endl;
  return 0;
}
{% endhighlight %}

字串記憶體位址可以由main函式(調用者)，傳入記憶體位址參數給func1函式<br>
{% highlight c++ linenos %}
char* func(char* c1, char* c2) {
  if (strlen(c1) > strlen(c2))
    return c1;
  else
    return c2;
}
int main() {
  char c1[100] = "Hello World!";
  char c2[100] = "ABCDEF";
  char* c = func(c1, c2);
  cout << c << endl;
  return 0;
}
{% endhighlight %}
```
Hello World!
```

## 結論
傳回記憶體位址的方式如下:
1. main()函式，傳入記憶體位址參數給函式()。
2. 使用static靜態變數。
3. 字串可以用RODATA區塊中的字串常數。
4. 動態分配記憶體，要記得delete與nullptr。
5. 全域變數。
6. 使用「傳值」更安全。


