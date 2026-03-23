---
title: 函式參數為指標的指標
date: 2026-03-22
keywords: c++, pointer to pointer pass to a function
---
Prerequisites:

- [指標的指標][3]

## 函式參數為指標
函式參數為指標，就是「複製」記憶體位址給「參數」。<br>

在main()主函式中，建立Student s物件，s物件記憶體位址為0x1000<br>
![img]({{site.imgurl}}/c++/point_param1.png)<br>

1.呼叫callByPoint()函式，並把0x1000記憶體位址作為參數傳遞過去。<br>
2.在Stack中，建立callByPoint()函式的空間。<br>
建立`新指標變數s`，把傳過來的0x1000，指派給`新指標變數s`。<br>
此時 main() 主函式中的s變數與 callByPoint() 函式中的`新指標變數s`，都指向0x1000。<br>
![img]({{site.imgurl}}/c++/point_param2.png)<br>

callByPoint() 函式中的`新指標變數s`，指向nullptr(記憶體位址0x0000)。<br>
![img]({{site.imgurl}}/c++/point_param3.png)<br>

離開callByPoint()函式，callByPoint()函式的記憶體位址被釋放，包含callByPoint() 函式中的`新指標變數s`記憶體位址也被釋放。<br>
![img]({{site.imgurl}}/c++/point_param4.png)<br>

所以main()主函式的Student s物件，s物件記憶體位址一直為0x1000<br>

### 完整程式碼
以下的程式碼中，建立物件s，並把它的記憶體位址傳入callByPoint()的函式中。<br>
試圖把記憶體位址修改成nullptr，在函式中確實是有修改成nullptr。<br>
但離開函式後，s的記憶體位址沒有修改。<br>
{% highlight python linenos %}
#include <iostream>
#include "main.h"
using namespace std;
class Student {
 public:
  const char* name_;
};
void callByPoint(Student* s) {
  cout << "callByPoint before address = " << s << endl;
  s = nullptr;
  cout << "callByPoint after address = " << s << endl;
}

int main() {
  Student s;
  cout << "Before main() address = " << &s << endl;
  callByPoint(&s);
  cout << "After main() address = " << &s << endl;
  return 0;
}
{% endhighlight %}
```
Before main() address = 0x7ff7bfeff2b0
callByPoint before address = 0x7ff7bfeff2b0
callByPoint after address = 0x0
After main() address = 0x7ff7bfeff2b0
```

## 函式參數為指標的指標
函式參數為指標的指標，就可以修改外部指標變數的記憶體位址。<br>

函式參數為「指標的指標」語法:
```
傳回型態 函式名(指標資料型態** 指標) {
  *指標 = 修改其它記憶體位址
}
```
使用2個星號`**`宣告這個是「指標的指標」，「指標的指標」存放的是指標的記憶體位址。


呼叫函式語法:
```
建立物件
指標 = &物件記憶體位址
函式(&指標位址)
```
要先建立一個指標，這個指標指向的是物件的記憶體位址。<br>
再把「指標的位址」，傳入函式。<br>

在main()主函式中，建立Student s物件，s物件記憶體位址為0x1000<br>
![img]({{site.imgurl}}/c++/pp_param1.png)<br>

指標變數ptr，ptr的記憶體位址是0x2000，儲存的值為0x1000，0x1000指向s物件。<br>
![img]({{site.imgurl}}/c++/pp_param2.png)<br>

1.建立callByPPoint()函式空間。<br>
2.建立s指標，儲存的是ptr指標0x2000。<br>
![img]({{site.imgurl}}/c++/pp_param3.png)<br>

將0x2000的內容，原本是0x1000，改成0x0000 nullptr的位址。<br>
![img]({{site.imgurl}}/c++/pp_param4.png)<br>

離開callByPPoint()，s指標記憶體位址會被釋放。<br>
![img]({{site.imgurl}}/c++/pp_param5.png)<br>

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include "main.h"
using namespace std;
class Student {
 public:
  const char* name_;
};

void callByPPoint(Student** s) {
  cout << "callByPPoint before address = " << *s << endl;
  *s = nullptr;
  cout << "callByPPoint after address = " << *s << endl;
}

int main() {
  Student s;
  Student* ptr = &s;
  cout << "Before main() address = " << ptr << endl;
  callByPPoint(&ptr);
  cout << "After main() address = " << ptr << endl;
  return 0;
}
{% endhighlight %}
```
Before main() address = 0x7ff7bfeff2b0
callByPPoint before address = 0x7ff7bfeff2b0
callByPPoint after address = 0x0
After main() address = 0x0
```

### 使用new 建立物件
建立使用new動態記憶體建立，建立時是回傳物件存放的記憶體位址，所以使用Student類型的指標接收。<br>
```
Student* s = new Student();
```
使用new 建立物件，物件存放的位址在Heap中，而不是在Stack中。<br>

因為s指標本身就是記憶體位址，所以傳入參數為「指標的指標」時，只要傳入指標的位址就好。<br>
```
callByPPoint(&s);
```
在Heap中建立student物件，使用new，傳回的是記憶體位址0x1000，使用Student指標去接收傳回的記憶體位址。<br>
![img]({{site.imgurl}}/c++/new_pp_param1.png)<br>

1.在Stack中建立callByPPoint的函式空間<br>
2.複製s指標的位址0x1000給callByPPoint()函式中的s指標。<br>
![img]({{site.imgurl}}/c++/new_pp_param2.png)<br>

將0x1000記憶體位址所儲存的內容，改為指向0x0000 nullptr<br>
![img]({{site.imgurl}}/c++/new_pp_param3.png)<br>

離開callByPPoint函式，callByPPoint()函式被記憶體回收。<br>
但main()函式中的s指標的內容已經指向nullptr。<br>
![img]({{site.imgurl}}/c++/new_pp_param4.png)<br>

完整程式碼
{% highlight c++ linenos %}
#include <iostream>
#include "main.h"
using namespace std;
class Student {
 public:
  const char* name_;
};

void callByPPoint(Student** s) {
  cout << "callByPPoint before address = " << *s << endl;
  *s = nullptr;
  cout << "callByPPoint after address = " << *s << endl;
}

int main() {
  Student* s = new Student();
  cout << "Before main() address = " << s << endl;
  callByPPoint(&s);
  cout << "After main() address = " << s << endl;
  return 0;
}
{% endhighlight %}
```
Before main() address = 0x600000008030
callByPPoint before address = 0x600000008030
callByPPoint after address = 0x0
After main() address = 0x0
```
-------------------------------------------------------
以前舊文章內容:<br>

## 函式參數為指標的指標

在函式中若要修改指標所指向的記憶體位址，就要用到指標的指標。

[引數][1]為「指標」的位址，函式參數宣告為**指標資料型態\*\* 指標變數**，這樣才可以接收指標的記憶體位址。

參考以下文章

<https://www.geeksforgeeks.org/passing-reference-to-a-pointer-in-c/>

> If a pointer is passed to a function as a parameter and tried to be modified then the changes made to the pointer does not reflects back outside that function. This is because only a copy of the pointer is passed to the function. It can be said that “pass by pointer” is passing a pointer by value. In most cases, this does not present a problem. But the problem comes when you modify the pointer inside the function. Instead of modifying the variable, you are only modifying a copy of the pointer and the original pointer remains unmodified.

> (google翻譯)如果將指標作為參數傳遞給函式並嘗試對其進行修改，則對指標所做的更改不會反映回該函式外部。這是因為僅將指標的副本傳遞給函式。可以說「透過指標傳遞」就是按值傳遞指標。在大多數情況下，這不會出現問題。但當你修改函式內部的指標時，問題就來了。您只是修改指標的副本，而不是修改變數，而原始指標保持不變。

- 函式語法

```
傳回型態 函式名(指標資料型態** 指標) {
  *指標 = 其它記憶體位址
}
```

- 呼叫函式語法
```
函式(&指標位址)
```

### 程式碼

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
int global_var = 100;
void changePointerValue(int** ptr_ptr){
  *ptr_ptr = &global_var; //改為指向global_var
}
int main() {
  int var = 1;
  int* pointer_to_var = &var; //指向var
  cout << "Before:" << *pointer_to_var << endl;
  //passing the address of the pointer
  //把指標的位址傳進函式中
  changePointerValue(&pointer_to_var);
  cout << "After:" << *pointer_to_var << endl;
  return 0;
}
{% endhighlight %}

```
Before:1
After:100
```


## 指標的指標與new

參考文章

[參考指向指標與new][2]

new會返回動態配置記憶體的開始位址，將p_to_p使用\*取值運算子修改p指向的位址。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
//宣告一個函式initAddress() 指標是p_to_p，指向外部指標的位址
void initAddress(int** p_to_p){
  //印出指向外部指標p的記憶體位址
  cout << "Before p address = " << *p_to_p << endl;
  //動態配置記憶體位址，位址存放的內容為10，使用new會返回動態配置記憶體的開始位址。
  //使用*取值運算子修改指標p指向的位址
  *p_to_p = new int(10);
  //印出指向外部指標的記憶體位址與值
  cout  << "After p address= " << *p_to_p << ",After p value = " << **p_to_p << endl;
}
int main() {
  //宣告指標p，初始化為nullptr，也就是沒有指向任何位址
  int* p = nullptr;
  //呼叫函式initAddress，引數為指標p的位址
  initAddress(&p);
  //印出指標p的位址，印出指標p指向的位址，對指向的位址取出內容。
  cout << "== outside == " << endl;
  cout << "outside pointer address = " << p << "，outside pointer value = " << *p << endl;
  return 0;
}
{% endhighlight %}

```
Before p address = 0x0
After p address= 0x60000000c010,After p value = 10
== outside == 
outside pointer address = 0x60000000c010，outside pointer value = 10
```

[1]: {% link _pages/c/basic/param.md %}
[2]: {% link _pages/c/reference/refToPointer.md %}
[3]: {% link _pages/c/pointer/pointerToPointer.md %}