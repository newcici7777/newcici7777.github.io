---
title: 參考指向指標
date: 2024-06-20
keywords: c++, a reference to a pointer
---

Prerequisites:

- [指標的指標][1]
- [引數][2]

參考指向指標，代表參考可以像指標一樣，使用\*取值運算子，取出原指標指向的記憶體位址中的值，也可以修改原指標指向的記憶體位址中的值，或更改原指標指向的記憶體位址。

## 宣告參考
```
資料型態*& 別名 = 原指標;
```
{% highlight c++ linenos %}
  int i = 10;
  cout << "i address:" << &i << endl;
  int j = 100;
  cout << "j address:" << &j << endl;

  //宣告指標
  int* ptr_i = &i;

  //宣告參考
  // 原指標指派給參考
  int*& ptr_ref = ptr_i;
  cout << "== Change value ==" << endl;
  cout << "Before i value:" << i << endl;
  cout << "Before value:" << *ptr_ref << endl;
  //修改原指標指向的記憶體位址中的值
  *ptr_ref = 20;
  cout << "After i value:" << i << endl;
  //取出原指標指向的記憶體位址中的值
  cout << "After value:" << *ptr_ref << endl;
  
  cout << "== Change address ==" << endl;
  cout << "Before address:" << ptr_ref << endl;
  //更改原指標指向的記憶體位址
  ptr_ref = &j;
  cout << "after address:" << ptr_ref << endl;
  return 0;
{% endhighlight %}

```
i address:0x7ff7bfeff468
j address:0x7ff7bfeff464
== Change value ==
Before i value:10
Before value:10
After i value:20
After value:20
== Change address ==
Before address:0x7ff7bfeff468
after address:0x7ff7bfeff464
```

參考以下文章

<https://www.geeksforgeeks.org/different-ways-to-use-const-with-reference-to-a-pointer-in-c/?ref=ml_lbp>

>References to pointers is a modifiable value that’s used same as a normal pointer.
>(google翻譯)對指標的參考是一個可修改的值，其使用方式與普通指標相同。


## 函式參數為指向指標的參考

如果要在函式中修改指標指向的記憶體位址，就需要使用[指標的指標][1]，也可以使用參考。

[引數][2]為指標，函式參數宣告為**指標資料型態\*& 別名**，這樣才可以修改指標指向的記憶體位址。

參考以下文章

<https://www.geeksforgeeks.org/passing-reference-to-a-pointer-in-c/>

> If a pointer is passed to a function as a parameter and tried to be modified then the changes made to the pointer does not reflects back outside that function. This is because only a copy of the pointer is passed to the function. It can be said that “pass by pointer” is passing a pointer by value. In most cases, this does not present a problem. But the problem comes when you modify the pointer inside the function. Instead of modifying the variable, you are only modifying a copy of the pointer and the original pointer remains unmodified.

> (google翻譯)如果將指標作為參數傳遞給函數並嘗試對其進行修改，則對指標所做的更改不會反映回該函數外部。這是因為僅將指標的副本傳遞給函數。可以說「透過指標傳遞」就是按值傳遞指標。在大多數情況下，這不會出現問題。但當你修改函數內部的指標時，問題就來了。您只是修改指標的副本，而不是修改變量，而原始指標保持不變。


### 語法

函式語法

```
回傳型態 函式名(資料型態*& 別名) {
  別名 = 其它記憶體位址
}
```

呼叫函式語法

```
函式(指標)
```

### 程式碼

{% highlight c++ linenos %}
int global_var = 100;
// a function with “Reference to pointer” parameter
void changeReferenceValue(int*& ptr_ptr){
  ptr_ptr = &global_var;
}
int main() {
  int var = 1;
  int* pointer_to_var = &var;
  cout << "Before:" << *pointer_to_var << endl;
  //把指標變數名傳進函式中
  changeReferenceValue(pointer_to_var);
  cout << "After:" << *pointer_to_var << endl;
  return 0;
}
{% endhighlight %}

```
Before:1
After:100
```

### 函式參數`指標的指標`與`指標的別名`程式差異

#### 指標的別名

<pre>
int global_var = 100;
// a function with “Reference to pointer” parameter
void changeReferenceValue(<span class="markline">int*&</span> ptr_ptr){
  <span class="markline">ptr_ptr</span> = &global_var;
}
int main() {
  int var = 1;
  int* pointer_to_var = &var;
  cout << "Before:" << *pointer_to_var << endl;
  //把指標變數名傳進函式中
  changeReferenceValue(<span class="markline">pointer_to_var</span>);
  cout << "After:" << *pointer_to_var << endl;
  return 0;
}

</pre>

#### 指標的指標

<pre>
int global_var = 100;
void changePointerValue(<span class="markline">int**</span> ptr_ptr){
  <span class="markline">*ptr_ptr</span> = &global_var; //改為指向global_var
}
int main() {
  int var = 1;
  int* pointer_to_var = &var; //指向var
  cout << "Before:" << *pointer_to_var << endl;
  //passing the address of the pointer
  //把指標的位址傳進函式中
  changePointerValue(<span class="markline">&pointer_to_var</span>);
  cout << "After:" << *pointer_to_var << endl;
  return 0;
}
</pre>

## 參考指向指標與new

參考文章

[指標的指標與new][3]

new會返回動態配置記憶體的開始位址，將別名指向new返回的新位址。

{% highlight c++ linenos %}
#include <iostream>
using namespace std;
//宣告一個函式initAddress() 別名是ref_to_p，指向外部傳入的指標
void initAddress(int*& ref_to_p){
  //印出別名指向的記憶體位址
  cout << "Before address = " << ref_to_p << endl;
  //動態配置記憶體位址，位址存放的內容為10，使用new會返回動態配置記憶體的開始位址。
  ref_to_p = new int(10);
  //印出別名指向的記憶體位址
  cout  << "After address= " << ref_to_p << ",After value = " << *ref_to_p << endl;
}
int main() {
  //宣告指標p，初始化為nullptr，也就是沒有指向任何位址
  int* p = nullptr;
  //呼叫函式initAddress，引數為指標p
  initAddress(p);
  //印出指標p的位址，印出指標p指向的位址，對指向的位址取出內容。
  cout << "== outside == " << endl;
  cout << "outside pointer address = " << p << "，outside pointer value = " << *p << endl;
  return 0;
}
{% endhighlight %}

```
Before address = 0x0
After address= 0x600000004010,After value = 10
== outside == 
outside pointer address = 0x600000004010，outside pointer value = 10
```

[1]: {% link _pages/c/pointer/pointerToPointer.md %}#函式參數為指標的指標
[2]: {% link _pages/c/basic/param.md %}
[3]: {% link _pages/c/pointer/pointerToPointer.md %}#指標的指標與new