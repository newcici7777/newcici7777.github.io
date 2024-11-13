---
title: l-value與r-value
date: 2024-06-22
keywords: c++, l-value, r-value
---

```
l-value = r-value;
```

在等號左邊的叫l-value，在等號右邊的叫r-value。

區別方式

- 有名字一律為左值l-value，沒有名字一律為右值r-value
- 可以取得記憶體位址一律為左值，沒有辦法取得記憶體位址一律為右值


## l-value

> lvalue simply means an object that has an identifiable location in memory

可以放在等號左邊，並且可以被指派值都是l-value。

等號左邊(l-value)，也就是一個能夠擺在等號左邊的東西∶**一個變數，而非常數**。

等號左邊可以是變數、陣列元素、結構、[參考]({% link _pages/c/reference/reference.md %})、[取值運算子]({% link _pages/c/pointer/pointer.md %}#取值運算子)。

以下都是lavlue:

### 有定義資料型態(int, double, float, char, long long ...)的變數，可以`指派值`。

{% highlight c++ linenos %}
    // i is l-value
    int i;
    //j is l-value
    int j = 10; 	// 10 is r-value
{% endhighlight %}

### 有定義資料型態(int, double, float, char, long long ...)的指標，可以指向記憶體位址。

Prerequisites:

[指標基本觀念][1]

{% highlight c++ linenos %}
    //p1 is l-value
    int* p1;
    //p2 is l-value
    int* p2 = &j;	//&j is r-value
{% endhighlight %}

### 可以用\*取值運算子修改指標指向的記憶體位址的`內容`

{% highlight c++ linenos %}
    //j is l-value
    int j = 10; //  10 is r-value
    
    //p2 is l-value
    int* p2 = &j;   //&j is r-value
    
    //*p2 is lavlue
    *p2 = 100;  //100 is r-value
{% endhighlight %}

### 可以將變數或指標，指派給&參考

Prerequisites:

[參考][5]

[參考指向指標][6]

#### 參考變數

{% highlight c++ linenos %}
    // i is l-value
    int i = 10; // 10 is r-value
    // ref is l-value
    int& ref = i;
{% endhighlight %}

#### 參考指標

{% highlight c++ linenos %}
    // i is l-value
    int i = 10; // 10 is r-value
    
    //宣告指標
    // ptr_i is l-value
    int* ptr_i = &i; // &i is r-value

    //宣告參考
    // 指標指派給參考
    // ref_to_ptr is l-value
    int*& ref_to_ptr = ptr_i; // ptr_i is l-value
{% endhighlight %}

### 陣列

Prerequisites:

[陣列][2]

{% highlight c++ linenos %}
	// arr is l-value
    int arr[] = {0,1,2,3}; // 0,1,2,3 is r-value
{% endhighlight %}

#### 陣列元素

{% highlight c++ linenos %}
int main() {
    // str is l-value
    char str[6];
    // str[0] is l-value
    str[0] = 'H'; // H is r-value
    // str[1] is l-value
    str[1] = 'e'; // e is r-value
    // str[2] is l-value
    str[2] = 'l'; // l is r-value
    // str[3] is l-value
    str[3] = 'l'; // l is r-value
    // str[4] is l-value
    str[4] = 'o'; // o is r-value
    // str[5] is l-value
    str[5] = '\0'; // \0 is r-value
    cout << str << endl;
    return 0;
}
{% endhighlight %}

```
Hello
```

#### *(陣列名 + 索引)

Prerequisites:

[一維陣列與指標][3]

{% highlight c++ linenos %}
int main() {
    // str is l-value
    char str[6];
    //*(str + 0) is l-value
    *(str + 0) = 'H'; // H is r-value
    //*(str + 1) is l-value
    *(str + 1) = 'E'; // E is r-value
    //*(str + 2) is l-value
    *(str + 2) = 'L'; // L is r-value
    //*(str + 3) is l-value
    *(str + 3) = 'L'; // L is r-value
    //*(str + 4) is l-value
    *(str + 4) = 'O'; // O is r-value
    //*(str + 5) is l-value
    *(str + 5) = '\0'; // \0 is r-value
    cout << str << endl;
    return 0;
}
{% endhighlight %}
```
HELLO
```

#### 指標指向陣列

Prerequisites:

[一維陣列與指標][3]

{% highlight c++ linenos %}
    // array is l-value
    int array[5];
    // p is l-value
    int* p = array; // array is l-value
{% endhighlight %}


#### const與指標

Prerequisites:

[const與指標][7]

{% highlight c++ linenos %}
    //var1 is l-value
    int var1 = 10; // 10 is r-value
    // p is l-value
    const int* p = &var1; // &var1 is r-value
{% endhighlight %}


## r-value

> r-value” refers to data value that is stored at some address in memory. 

等號右邊的東西可以是[字串常數]({% link _pages/c/array/pointerCharArr.md %}#字串常數)、表達式。

以下是r-value:

### 字串常數

Prerequisites:

[char字串][4]

{% highlight c++ linenos %}
int main() {
    // cstr2 is l-value
    char cstr2[] = "hello"; // hello is r-value
    //cstr3 is l-value
    char cstr3[6] = "hello";// hello is r-value
    //cstr4 is l-value
    char cstr4[] = {"hello"};// hello is r-value
    //cstr5 is l-value
    char cstr5[6] = {"hello"};// hello is r-value
    //cstr6 is l-value
    char cstr6[6] {"hello"};// hello is r-value
    return 0;
}
{% endhighlight %}

### &陣列 與 &陣列[索引] 

{% highlight c++ linenos %}
int main() {
    // array is l-value
    int array[5];
    // array is l-value
    cout << "陣列名 = " << array << endl;
    // &array is r-value
    cout << "陣列名地址 = " << &array << endl;
    // &array[0] is r-value
    cout << "array[0]地址 = " << &array[0] << endl;
    // &array[1] is r-value
    cout << "array[1]地址 = " << &array[1] << endl;
    // &array[2] is r-value
    cout << "array[2]地址 = " << &array[2] << endl;
    // &array[3] is r-value
    cout << "array[3]地址 = " << &array[3] << endl;
    return 0;
}
{% endhighlight %}

### 指標 + 整數(0,1,2,3 ...)

{% highlight c++ linenos %}
int main() {
    // array is l-value
    int array[5];
    // p is l-value
    int* p = array; // array is l-value
    // p is l-value
    cout << "p指標內容 = " << p << endl;
    //p + 0 is r-value
    cout << "p指標+0 = " << p + 0 << endl;
    //p + 1 is r-value
    cout << "p指標+1 = " << p + 1 << endl;
    //p + 2 is r-value
    cout << "p指標+2 = " << p + 2 << endl;
    //p + 3 is r-value
    cout << "p指標+3 = " << p + 3 << endl;
    return 0;
}
{% endhighlight %}

{% highlight c++ linenos %}
int main() {
    // array is l-value
    int array[5];
    // p is l-value
    int* p = array; // array is l-value
    // p is l-value
    p = p + 1; // p + 1 is r-value
    return 0;
}
{% endhighlight %}

### 函式傳回值為物件的值

- [函式傳回值是值][8]

以下的程式碼，main()和getStudente()都各別有存放回傳值student的記憶體位址，待getStudent()的student變數回傳給main()的student變數後，getStudent()的student變數的記憶體位址就會被釋放。

被記憶體釋放，不會被保留住的物件，也是右值r-value。

#### 傳回物件

{% highlight c++ linenos %}
Student getStudent() {
    Student student;
    return student;
}
int main() {
    Student student = getStudent();
    return 0;
}
{% endhighlight %}

#### 回傳值是臨時物件

- [臨時物件][9]

臨時物件是右值。

臨時物件建立語法
```
類別名()
Student()
```
{% highlight c++ linenos %}
Student getStudent() {
    return Student();
}
int main() {
    Student student = getStudent();
    return 0;
}
{% endhighlight %}

[1]: {% link _pages/c/pointer/pointer.md %}
[2]: {% link _pages/c/array/array.md %}
[3]: {% link _pages/c/array/pointerToArray.md %}
[4]: {% link _pages/c/array/charArray.md%}
[5]: {% link _pages/c/reference/reference.md%}
[6]: {% link _pages/c/reference/refToPointer.md%}
[7]: {% link _pages/c/pointer/pointerConst.md%}
[8]: {% link _pages/c/function/callByValue.md %}#函式傳回值是值
[9]: {% link _pages/c/class/temp_obj.md%}


